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
---
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

