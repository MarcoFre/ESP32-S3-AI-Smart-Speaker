# HackMyHome – ESP32-S3 AI Smart Speaker Development Board (Voice Assist O3)

Configurazione **ESPHome** per trasformare la **Waveshare ESP32-S3 Audio Board / ESP32-S3 AI Smart Speaker Development Board** in un **Voice Assistant** per Home Assistant, con wake word locale, TTS, media player, mixer audio con ducking e feedback visivo tramite LED ring.

---

## 🔗 Link utili

### 🛒 ESP32-S3 AI Smart Speaker Development Board
- **Waveshare (affiliate link):** https://www.waveshare.com/?&aff_id=HackMyHome  
- **Amazon (affiliate link):** https://amzn.to/40JLOQq

### 🌐 HackMyHome
- **GitHub (repo progetto):** https://github.com/MarcoFre/ESP32-S3-AI-Smart-Speaker/tree/main  
- **Facebook:** https://www.facebook.com/profile.php?id=61583821941006

---

## 🤝 Trasparenza (link di supporto)
Alcuni link (Waveshare/Amazon) sono **reference/affiliate link**: se li usi per acquistare, supporti il progetto **senza costi extra** per te.  
Le opinioni e i test restano indipendenti.

---

## ✅ Funzionalità principali

- ✅ **Wake word locale** (MicroWakeWord) – es: *Hey Jarvis / Hey Elsa / Hey Aura*
- ✅ **Conversazione continua** (`voice_assistant.start_continuous`)
- ✅ **Media player** con doppia pipeline:
  - `media_pipeline` (musica/stream)
  - `announcement_pipeline` (TTS/annunci)
- ✅ **Mixer audio + ducking**: abbassa la musica durante TTS/annunci e ripristina a fine annuncio
- ✅ **LED ring WS2812 (7 LED)** con stati e animazioni (standby/listening/thinking/speaking/error…)
- ✅ **Tasti fisici**: volume + / mute / volume –
- ✅ (Opzionale) **OLED SSD1306** con stato e animazioni (faccina)
- ✅ **Timing report** in log: tempi di thinking/speaking per capire dove “si perde tempo”

---

## ✅ Requisiti

### Hardware
- **Waveshare ESP32-S3 Audio Board** (ES7210 + ES8311, TCA9555, WS2812 ring)
- Cavo **USB-C dati** (per il primo flash)
- (Opzionale) OLED **SSD1306 128x64** su I²C (address `0x3C`)

### Software
- Home Assistant con integrazione **ESPHome**
- ESPHome aggiornato (consigliato **ESP-IDF** per audio/media)

---

## 🚀 Installazione (flash su ESP32)

### Metodo A — ESPHome Dashboard (consigliato)
1. Apri **ESPHome** (Add-on o Dashboard)
2. **New device**
3. Apri il file YAML del device e incolla la configurazione del progetto
4. Compila e carica:
   - prima volta: **Install via USB**
   - successivamente: **OTA** (via rete)

> Se il flash non parte: tieni premuto **BOOT**, premi **RESET**, rilascia **RESET**, poi rilascia **BOOT** (modalità download).

### Metodo B — ESPHome CLI
```bash
esphome run voice-assist-03.yaml

```
## 🔧 Configurazione iniziale (substitutions)

La parte “personale” del progetto è tutta concentrata nel blocco `substitutions`: qui metti **identità**, **rete**, **pin** e **tuning**.  
L’idea è semplice: *cloni il progetto su un altro device e cambi solo questi valori*.

### Identity
- `hostname` → nome tecnico ESPHome (anche hostname di rete)
- `friendly` → nome “umano” che vedrai in Home Assistant

### Security
- `password` → password Wi-Fi (se la usi direttamente qui; consigliato spostarla in `secrets.yaml`)
- `api_key` → chiave di cifratura dell’API ESPHome
- `ota_key` → password OTA per aggiornare senza cavo

### WiFi
- `ssid` → nome rete Wi-Fi
- `ip / gateway / subnet / dns` → IP statico (consigliato per un voice assistant: HA lo trova sempre allo stesso indirizzo)

### LED ring
- `led_pin` → pin dati WS2812 (di default `GPIO38`)
- `led_count` → numero LED (qui 7)

### PIN AUDIO/DISPLAY
- `i2c_sda / i2c_scl` → bus I²C (controllo codec + expander)
- `i2s_mclk / i2s_bclk / i2s_lrclk` → clock I²S
- `i2s_din / i2s_dout` → dati audio in/out (codec ↔ ESP32)

### AUDIO TUNING
- `sample_rate` → sample rate uscita audio (tipico `48000`)
- `bits_per_sample` → profondità (tipico `16bit`)
- `auto_stop_cycle` → quante “ripartenze” dopo `stt-no-text-recognized` prima di tornare idle

### LED Ring effects (timing + palette)
Qui imposti sia la **velocità** delle animazioni sia i **colori** dei vari stati.

**Timing**
- `led_rotate_ms` → velocità rotazione “listening”
- `led_standby_ms` → velocità standby

**Standby (two dots)**
- `standby_speed_ms`, `standby_gap`
- `standby_r1/g1/b1` e `standby_r2/g2/b2` → due pallini con colori diversi

**Listening / Thinking / Speaking / Error**
- `listen_*` → colore foreground/background in ascolto
- `think_*` → colore thinking
- `speak_*` → colore speaking
- `err_*` → colore error

### Phases (stati numerici)
Queste costanti sono gli ID che usi per pilotare LED e display in modo leggibile (eviti “numeri magici” sparsi nel codice):

**Device phases**
- `no_connected`, `connected_wireless`, `connected_wireless_api`

**Voice Assistant phases**
- `voice_assist_idle_phase_id`, `...listening...`, `...thinking...`, `...replying...`
- `...not_ready...`, `...error...`, `...muted...`, `...timer_finished...`

**MediaPlayer phases**
- `mediaplayer_play`, `mediaplayer_pause`, `mediaplayer_stop`

> In breve: `substitutions` è il pannello di controllo del progetto.  
> Tutto il resto del YAML lavora “a valle” usando questi valori.
## 🧠 Logica degli stati (globals + control_leds)

Questa configurazione usa una **state machine** semplice ma molto efficace: un singolo intero (`voice_assistant_phase`) rappresenta **lo stato corrente** del device e guida tutto il resto (LED ring, display, avvio/stop wake word, ripristini dopo annunci, ecc.).

### Variabili globali principali

- `voice_assistant_phase`  
  Stato corrente del dispositivo (offline / wifi / api / idle / listening / thinking / replying / play / mute / error / timer).

- `voice_assistant_phase_prev`  
  Ultimo stato “utile” prima di un cambio temporaneo (es. annunci o musica). Serve per **tornare allo stato corretto** a fine evento.

- `audio_muted`  
  Flag mute “logico” (oltre al mute del DAC). Quando è `true`:
  - la wake word non parte
  - lo stato diventa **Muted**
  - i tasti Volume smutano prima di cambiare volume

- Timestamp per misurare performance (ms):
  - `ts_va_listening_ms`
  - `ts_va_vad_end_ms`
  - `ts_va_stt_end_ms`
  - `ts_va_tts_start_ms`
  - `ts_va_reply_done_ms`
  - `ts_va_end_ms`

- `question` / `answer`  
  Ultima domanda riconosciuta e ultima risposta TTS (per log e display/telemetria).

---

## 🎛️ Avvio/stop Wake Word

La wake word è gestita in modo “controllato” per evitare conflitti audio e falsi trigger:

### `start_wake_word`
Avvia `micro_wake_word` **solo se**:
- non sei in mute (`!audio_muted`)
- il `voice_assistant` non è già in esecuzione
- `micro_wake_word` non è già in esecuzione

In questo modo, quando il VA sta parlando o la musica è in play, non lasci “ascolto” sempre acceso.

### `stop_wake_word`
Ferma `micro_wake_word` se è in esecuzione.  
Viene usato tipicamente quando:
- parte la musica (`on_play`)
- parte un annuncio (`on_announcement`)
- vai offline / perdi API
- entri in mute

---

## 🧩 `set_idle_or_mute_phase`

Piccolo ma fondamentale: decide lo stato “a riposo” in base al mute.

- se `audio_muted == true` → `voice_assistant_phase = MUTED`
- altrimenti → `voice_assistant_phase = IDLE`

Serve per ripristinare correttamente lo stato dopo:
- fine VA
- fine annuncio
- pause/stop musica

---

## 🎚️ Il regista: `control_leds`

`control_leds` è lo script centrale che traduce lo stato in **feedback visivo**.

### 1) Log e memorizzazione stato precedente
All’inizio stampa a log lo stato corrente e, per alcuni stati “buoni”, aggiorna `voice_assistant_phase_prev`.

Viene salvato come “previous” quando lo stato è uno tra:
- Idle
- Listening
- Thinking
- Replying
- MediaPlayer Play

Questo evita che stati transitori (offline/error/blink vari) sovrascrivano lo “stato di ritorno”.

### 2) Mappatura stato → effetto LED
Poi, in base a `voice_assistant_phase`, imposta un effetto sul LED ring:

- **Offline (`no_connected`)** → `Error Blink`
- **Wi-Fi ok (`connected_wireless`)** → `WiFi Blink`
- **API ok (`connected_wireless_api`)** → `API Blink`
- **Not ready** → `Error Blink`
- **Error** → `Error Blink`
- **Idle** → esegue `led_waiting_wake` → `VA Standby`
- **Listening** → esegue `led_listening` → `VA Rotate`
- **Thinking** → esegue `led_thinking` → `VA Think Pulse`
- **Replying** → `Play`
- **Media play** → `Slow Pulse`
- **Muted** → `Pause Blink`
- **Timer finished** → `Announce Blink`

---

## 🌈 Stati e LED Ring (mappa rapida)

| Stato | ID | Effetto LED |
|------|---:|------------|
| Nessuna connessione | `${no_connected}` (=0) | **Error Blink** |
| Wi-Fi connesso | `${connected_wireless}` (=1) | **WiFi Blink** |
| API connessa | `${connected_wireless_api}` (=2) | **API Blink** |
| Idle | `${voice_assist_idle_phase_id}` (=11) | **VA Standby** |
| Listening | `${voice_assist_listening_phase_id}` (=12) | **VA Rotate** |
| Thinking | `${voice_assist_thinking_phase_id}` (=13) | **VA Think Pulse** |
| Speaking/Replying | `${voice_assist_replying_phase_id}` (=14) | **Play** |
| Media Play | `${mediaplayer_play}` (=21) | **Slow Pulse** |
| Muted | `${voice_assist_muted_phase_id}` (=17) | **Pause Blink** |
| Timer finished | `${voice_assist_timer_finished_phase_id}` (=18) | **Announce Blink** |
| Error | `${voice_assist_error_phase_id}` (=16) | **Error Blink** |

> Colori e velocità (foreground/background e timing) sono tutti personalizzabili nel blocco `substitutions`.

---

## 🌀 Effetti “VA” (standby / listening / thinking)

### `VA Standby`
Effetto standby: **due dot** che ruotano, con gap e colori configurabili:
- velocità: `standby_speed_ms`
- distanza: `standby_gap`
- colori: `standby_r1/g1/b1` + `standby_r2/g2/b2`

### `VA Rotate` (Listening)
Imposta prima i colori (fg/bg) prendendoli da `listen_*` e poi avvia una rotazione singolo dot su background.

### `VA Think Pulse` (Thinking)
Imposta i colori da `think_*` e crea un pulse su due punti opposti dell’anello.

---

## ⏱️ Timing report (va_timing_report)

A fine conversazione, lo script `va_timing_report` calcola e stampa:
- **Totale**: STT_END → SPEECH_END
- **Thinking**: STT_END → TTS_START
- **Speaking**: TTS_START → SPEECH_END
e anche i tempi “pre”:
- listening → VAD_END
- VAD_END → STT_END
e “post”:
- speech_end → end(cleanup)

È utile per capire se il collo di bottiglia è:
- STT/intent/AI
- TTS
- oppure playback / rete / CPU
