# HackMyHome – ESP32-S3 AI Smart Speaker Development Board
Configurazione **ESPHome** per trasformare la **Waveshare ESP32-S3 Audio Board / ESP32-S3 AI Smart Speaker Development Board** in un **Voice Assistant evoluto per Home Assistant**, con wake word locale, TTS, media player, mixer audio, display circolare, LED ring, timer, sveglia, modalità antifurto, controlli fisici e controlli da Home Assistant.

---

Il progetto nasce per la serie **HackMyHome Voice Assistant** e usa una logica a stati per coordinare audio, microfono, wake word, display, LED ring, timer, sveglia, antifurto, media player e pulsanti fisici.

---

## 🔖 Versione
**Firmware:** `5.1.0`

---

## 🆕 Novità della versione `5.1.0`
La versione **5.1.0** consolida il progetto come piattaforma completa per sperimentare con Voice Assistant, display, audio e automazioni locali.
### Nuove funzioni principali
* **Local intent direttamente su ESPHome**
  * riconoscimento comandi locali da testo STT
  * volume su/giù senza passare dall’agente conversazionale
  * gestione antifurto da comando vocale
  * stop locale del Voice Assistant tramite comando “modalità silenziosa”
* **Display GC9A01A evoluto**
  * animazioni della faccina più fluide
  * sleep automatico
  * risveglio tramite rumore ambientale
  * modalità musica con sfondo dinamico reattivo all’audio
  * VU meter laterale
  * barra volume laterale
  * badge a display per nuovo firmware disponibile
* **Gestione audio migliorata**
  * mixer con pipeline separata per media e annunci
  * ducking automatico durante Voice Assistant, TTS, timer e sveglia
  * sound level meter integrato
  * tuning del microfono da Home Assistant
* **Timer e sveglia**
  * visualizzazione timer con arco circolare
  * nome del timer sul display
  * suono timer ripetuto
  * sveglia configurabile da Home Assistant
  * evento `esphome.alarm_ringing`
* **Modalità antifurto locale**
  * switch da Home Assistant
  * rilevamento rumore tramite microfono
  * soglia configurabile
  * effetto LED Police
  * allarme locale con suono ripetuto
  * pulsante di test
* **Firmware check**
  * controllo automatico ogni 24 ore
  * controllo manuale da Home Assistant
  * badge sul display quando è disponibile una nuova versione

---

## 🔗 Link utili
### 🛒 ESP32-S3 AI Smart Speaker Development Board
* **Waveshare:** [https://www.waveshare.com/?&aff_id=HackMyHome](https://www.waveshare.com/?&aff_id=HackMyHome)
* **Amazon:** [https://amzn.to/40JLOQq](https://amzn.to/40JLOQq)
### 🛒 Display LCD IPS 1,28" GC9A01 / GC9A01A
* **Amazon:** [https://amzn.to/4ckpC4M](https://amzn.to/4ckpC4M)
### 🌐 HackMyHome
* [**ESP32-S3 AI Smart Speaker: da “all-in-board” a "Voice Assistant" - Home Assistant**](https://youtu.be/Lcp8FQ9NLyo)
* [**Aggiungiamo un Display Circolare! 📺✨**](https://youtu.be/EsESK5aimu4)
* [**YouTube: HackMyHome**](https://www.youtube.com/@HMH-Aura-Nova)
* [**Facebook: HackMyHome**](https://www.facebook.com/profile.php?id=61583821941006)

---

## 🤝 Trasparenza
Alcuni link Waveshare/Amazon possono essere **reference link o affiliate link**. Se li usi per acquistare, supporti il progetto **senza costi extra** per te.
Le opinioni, i test e le configurazioni restano indipendenti.

---

## ✅ Funzionalità principali

### Voice Assistant
* Wake word locale tramite **MicroWakeWord**
* Supporto a più modelli wake word
* Wake word di stop interna
* Modalità **conversazione singola**
* Modalità **ascolto continuo**
* Avvio/stop del Voice Assistant tramite action ESPHome esposte in Home Assistant
* Gestione automatica della wake word dopo annunci, timer, musica e fine conversazione
* Stop automatico della modalità continua dopo inattività
* Invio eventi Home Assistant con testo STT e URI TTS
* Gestione local intent direttamente su ESPHome per alcune azioni rapide

### Local intent
La versione `5.1.0` introduce un primo livello di comandi locali basati sul testo STT, così alcune azioni possono essere gestite direttamente dal firmware senza passare dall’agente conversazionale.
Comandi previsti:
| Intent locale | Esempi frase | Azione |
| ------------ | ------------ | ------ |
| Volume su | “alza volume”, “aumenta volume” | Aumenta volume media player |
| Volume giù | “abbassa volume”, “riduci volume” | Riduce volume media player |
| Modalità silenziosa | “attiva modalità silenziosa” | Ferma il Voice Assistant locale |
| Antifurto on | “attiva antifurto”, “inserisci antifurto” | Attiva modalità antifurto |
| Antifurto off | “disattiva antifurto”, “disinserisci antifurto” | Disattiva modalità antifurto |
Questa logica è utile per comandi immediati, dove la priorità è la risposta locale e non l’elaborazione AI.

### Audio e media player
* Codec audio **ES7210** per microfono
* DAC **ES8311** per speaker
* Speaker I2S esterno
* Media player ESPHome integrato
* Pipeline separata per:
  * media/musica
  * annunci/TTS
* Mixer audio con ducking
* Ripristino volume/mixing dopo annunci, timer e Voice Assistant
* File audio integrati per:
  * mute on/off
  * wake word detected
  * timer finished

### Display GC9A01A
* Display circolare SPI **GC9A01A 240×240**
* Aggiornamento rapido del display
* Interfaccia grafica animata
* Faccina con occhi, bocca ed espressioni
* Emozioni selezionabili da Home Assistant:
  * Auto
  * Felice
  * Assonnato
  * Triste
  * Arrabbiato
  * Sorpreso
* Sleep automatico del display
* Risveglio del display tramite rumore ambientale
* Modalità musica con sfondo dinamico reattivo all’audio
* VU meter laterale
* Barra volume laterale
* Visualizzazione timer con arco circolare
* Badge a display se è disponibile un nuovo firmware
* Modalità antifurto con faccina/allarme visivo

### LED ring WS2812
* LED ring WS2812 su GPIO38
* 7 LED configurabili
* Effetti inclusi:
  * Police
  * Pulse Slow
  * Pulse Medium
  * Pulse Fast
  * Rainbow Slow
  * Wipe
* Luminosità configurabile da Home Assistant
* Colore LED impostabile tramite action ESPHome
* Feedback LED per:
  * boot
  * Wi-Fi/API
  * ascolto
  * thinking
  * risposta
  * musica
  * annunci
  * timer
  * antifurto
  * errore

### Timer e sveglia
* Supporto ai timer del Voice Assistant
* Visualizzazione del prossimo timer attivo
* Nome timer visualizzato sul display
* Ring sonoro del timer
* Stop timer tramite wake word di stop
* Sveglia configurabile da Home Assistant
* Azioni sveglia configurabili:
  * riproduci suono
  * invia evento
  * suono ed evento
* Gestione timezone tramite action ESPHome

### Modalità antifurto
* Switch Home Assistant per attivare/disattivare la modalità antifurto
* Sensore di rumore ambientale
* Soglia rumore configurabile
* Allarme locale con suoni ripetuti
* Effetto LED Police
* Display in modalità startled/allarme
* Pulsante di test antifurto

### Tuning audio e microfono
* Guadagno microfono ES7210 configurabile da Home Assistant
* Gain factor del microfono Voice Assistant configurabile
* Noise suppression configurabile
* Mute microfono da Home Assistant
* Sound level meter integrato:
  * RMS interno
  * Peak interno
  * sensore rumore in dB
* Wake word sensitivity selezionabile:
  * Poco sensibile
  * Medio
  * Molto sensibile

### Firmware check
* Controllo versione firmware da GitHub
* Verifica automatica ogni 24 ore
* Pulsante manuale di controllo firmware
* Switch per abilitare/disabilitare il controllo
* Notifica visiva sul display quando è disponibile un aggiornamento

---

## ✅ Requisiti

### Hardware
* Waveshare ESP32-S3 Audio Board / ESP32-S3 AI Smart Speaker Development Board
* Display SPI GC9A01A 1,28" 240×240
* Cavo USB-C dati per il primo flash

### Software
* Home Assistant
* Add-on ESPHome o ESPHome Dashboard
* ESPHome `2025.8.0` o superiore
* Framework ESP-IDF
* Integrazione Voice Assistant configurata in Home Assistant
* Pipeline Assist configurata con STT/TTS o agente conversazionale

---

## 🚀 Installazione

### Metodo A — ESPHome Dashboard

1. Apri ESPHome in Home Assistant.
2. Crea un nuovo device.
3. Incolla il file YAML del progetto.
4. Modifica il blocco `substitutions` con i tuoi valori.
5. Compila.
6. Esegui il primo flash via USB.
7. Dopo il primo flash puoi aggiornare via OTA.
8. 
Se il flash USB non parte, prova la modalità download:
1. tieni premuto **BOOT**
2. premi **RESET**
3. rilascia **RESET**
4. rilascia **BOOT**
5. avvia il flash

### Metodo B — ESPHome CLI
```bash
esphome run voice-assistant.yaml
```

---

## 🔧 Configurazione iniziale

La configurazione personale è concentrata nel blocco `substitutions`.
Esempio:
```yaml
substitutions:
  device_name: home-assistant-voice-speaker
  friendly: "Home Assistant Voice Speaker"
  fw_version: "5.1.0"
  password: "WiFi Password"
  api_key: "API encryption key"
  ota_key: "OTA password"
  ssid: "WiFi SSID"
  ip: "192.168.xxx.xxx"
  gateway: "192.168.xxx.xxx"
  subnet: "255.255.255.0"
  dns: "192.168.xxx.xxx"
```

### Identity
| Campo | Descrizione |
| ----- | ----------- |
| `device_name` | Nome tecnico ESPHome e hostname del device |
| `friendly` | Nome leggibile mostrato in Home Assistant |
| `fw_version` | Versione firmware mostrata dal progetto ESPHome e usata dal firmware check |

### Security
| Campo | Descrizione |
| ----- | ----------- |
| `password` | Password Wi-Fi |
| `api_key` | Chiave di cifratura API ESPHome |
| `ota_key` | Password OTA |

### Wi-Fi
| Campo | Descrizione |
| ----- | ----------- |
| `ssid` | Nome rete Wi-Fi |
| `ip` | IP statico del device |
| `gateway` | Gateway di rete |
| `subnet` | Subnet mask |
| `dns` | DNS principale |

Il progetto usa anche `8.8.8.8` come DNS secondario.

---

## 🧩 Pinout principale

### Display GC9A01A
| Funzione | GPIO |
| -------- | ---: |
| CS | GPIO3 |
| CLK/SCK | GPIO4 |
| DC | GPIO7 |
| MOSI | GPIO9 |

### I2C / Codec / IO expander
| Funzione | GPIO |
| -------- | ---: |
| I2C SCL | GPIO10 |
| I2C SDA | GPIO11 |

### I2S Audio
| Funzione | GPIO |
| -------- | ---: |
| MCLK | GPIO12 |
| BCLK | GPIO13 |
| LRCLK | GPIO14 |
| Microphone DIN | GPIO15 |
| Speaker DOUT | GPIO16 |

### LED ring
| Funzione | GPIO |
| -------- | ---: |
| WS2812 Data | GPIO38 |
| LED count | 7 |

### TCA9555 extended IO
| Funzione | Pin esteso |
| -------- | ---------: |
| PA_EN / amp control | 8 |
| RTC INT | 5 |
| K1 Volume - | 9 |
| K2 Play/Pausa | 10 |
| K3 Volume + | 11 |

---

## 🧠 State machine del Voice Assistant

Il progetto usa una variabile globale chiamata `voice_assistant_phase` per rappresentare lo stato corrente del dispositivo.
| Stato | ID | Descrizione |
| ----- | -: | ----------- |
| Idle | 1 | Pronto per la wake word |
| Waiting for command | 2 | Wake word rilevata, in attesa del comando |
| Listening for command | 3 | L'utente sta parlando |
| Thinking | 4 | Il comando è in elaborazione |
| Replying | 5 | Il sistema sta rispondendo |
| Not ready | 10 | Voice Assistant non pronto |
| Error | 11 | Errore |

Questa state machine pilota:
* LED ring
* display
* wake word
* ducking audio
* modalità musica
* antifurto
* timer/sveglia
* ripristino dello stato dopo annunci, musica, timer e conversazione

---

## 🎛️ Logica LED ring

Lo script centrale è `control_leds`. La priorità degli stati è:
1. Improv BLE in corso
2. Inizializzazione
3. Wi-Fi/API non disponibili
4. Timer in ringing
5. Antifurto con rumore rilevato
6. Musica in riproduzione
7. Annuncio/TTS in corso
8. Stato Voice Assistant
9. Timer attivi
10. Standby/off

### Mappa rapida LED
| Condizione | Effetto |
| ---------- | ------- |
| Improv BLE | Pulse Medium |
| Boot/init con Wi-Fi | Rainbow Slow |
| Boot/init senza Wi-Fi | Rainbow Slow rosso |
| Wi-Fi/API non disponibili | Pulse Medium rosso |
| Timer in ringing | Pulse Fast viola |
| Antifurto + rumore | Police |
| Media playing | Rainbow Slow |
| Announcement/TTS | Wipe |
| Waiting command | Solid viola |
| Listening | Pulse Medium viola |
| Thinking | Pulse Slow viola |
| Replying | Wipe |
| Error | Pulse Fast rosso |
| Not ready | Pulse Slow rosso |
| Timer attivo | Pulse Slow viola |
| Standby con antifurto off | LED spenti |

---

## 🖥️ Display GC9A01A
Il display è configurato come:
```yaml
display:
  - platform: mipi_spi
    model: GC9A01A
    update_interval: 50ms
    buffer_size: 100%
    color_depth: 16
    data_rate: 80MHz
    invert_colors: true
```
La lambda del display disegna una UI completa con:
* sfondo dinamico
* faccina animata
* occhi con movimento e blink
* bocca animata durante il parlato
* modalità sleep
* modalità musica reattiva all'audio
* VU meter laterale
* barra volume
* timer circolare
* badge aggiornamento firmware
* modalità antifurto

### Emozioni manuali
Da Home Assistant puoi forzare l'espressione del display tramite il select:

```text
Display - Emozione
```

Opzioni disponibili:
* Auto
* Felice
* Assonnato
* Triste
* Arrabbiato
* Sorpreso

---

## 🔊 Modalità musica
Quando il media player è in riproduzione e il Voice Assistant non sta ascoltando o rispondendo, il display entra in **music mode**. In questa modalità:
* lo sfondo cambia colore in modo dinamico
* il colore reagisce al livello audio
* vengono disegnate note musicali animate
* la faccina assume un'espressione più allegra
* il VU meter laterale mostra il livello audio
* i picchi audio generano brevi flash controllati

Il livello audio viene letto dal sensore `sound_level` tramite `playback_peak`.

---

## ⏱️ Timer

Il progetto supporta i timer del Voice Assistant. Quando un timer è attivo:
* viene pubblicato il sensore `Prossimo timer`
* viene pubblicato il text sensor `Prossimo timer - Nome`
* il display mostra un arco circolare con il tempo residuo
* il colore dell'arco cambia quando il timer si avvicina alla fine
* allo scadere parte il suono `timer_finished`
* la wake word di stop può interrompere il timer

Eventi gestiti:
* `on_timer_started`
* `on_timer_cancelled`
* `on_timer_updated`
* `on_timer_tick`
* `on_timer_finished`

---

## ⏰ Sveglia
La sveglia è gestita da entità Home Assistant esposte da ESPHome.
| Entità | Tipo | Descrizione |
| ------ | ---- | ----------- |
| `Sveglia attiva` | switch | Abilita/disabilita la sveglia |
| `Ora sveglia` | text sensor | Ora impostata in formato HH:MM |
| `Sveglia - Azione` | select | Azione da eseguire allo scatto |
| `Ora dispositivo` | text sensor | Ora corrente del dispositivo |

Azioni disponibili:
* Riproduci suono
* Invia evento
* Suono ed evento

Evento inviato:
```text
esphome.alarm_ringing
```

La sveglia può essere impostata anche tramite action ESPHome:
```yaml
action: esphome.home_assistant_voice_speaker_set_alarm_time
data:
  alarm_time_hh_mm: "07:30"
```

---

## 🛡️ Modalità antifurto
La modalità antifurto usa il microfono come sensore di rumore.
| Entità | Tipo | Descrizione |
| ------ | ---- | ----------- |
| `Modalità antifurto` | switch | Attiva/disattiva la modalità antifurto |
| `Rilevamento rumore` | binary sensor | Si attiva sopra la soglia configurata |
| `Soglia Rumore` | number | Soglia in dB |
| `Test antifurto` | button | Avvia un test locale |

Quando l'antifurto è attivo e viene rilevato rumore:
* parte lo script `antifurto_trigger`
* il display si sveglia
* viene mostrata una faccina startled/allarme
* il LED ring passa in modalità Police
* viene riprodotto il suono di allarme

Il rilevamento rumore è disabilitato automaticamente quando:
* il device sta già riproducendo musica
* è in corso un annuncio
* il timer sta suonando
* l'allarme antifurto è già in corso

---

## 🎙️ Microfono e tuning audio
Il progetto espone diversi controlli per regolare il comportamento audio senza ricompilare il firmware.

### Number disponibili
| Entità | Range | Descrizione |
| ------ | ----: | ----------- |
| `Soglia Rumore` | -75 / -25 dB | Soglia per risveglio display e antifurto |
| `Mic-Guadagno` | 0 / 42 dB | Guadagno ES7210 |
| `VA-Guadagno` | 1 / 64 | Gain factor del microfono VA |
| `Sopp. Rumore` | 0 / 4 | Noise suppression del Voice Assistant |
| `LED` | 0.4 / 1.0 | Luminosità LED ring |

### Sensori audio
| Entità | Descrizione |
| ------ | ----------- |
| `Rumore` | Livello rumore/peak in dB |
| `Playback RMS` | RMS interno, disabilitato di default |
| `Playback Peak` | Peak interno, usato da display e rilevamento rumore |

---

## 🔇 Mute microfono
Lo switch `Microfono disattivato`:
* ferma il Voice Assistant
* ferma MicroWakeWord
* imposta il gain del microfono a 0
* mostra un flash LED

Quando viene disattivato il mute:
* ripristina il gain microfono salvato
* mostra un flash LED
* prova a riavviare la wake word

---

## 🗣️ Wake word
Il progetto usa MicroWakeWord con più modelli:
* `okay_nabu`
* `kenobi`
* `hey_jarvis`
* `stop`, interno

La wake word `stop` è usata internamente per interrompere timer o annunci.

### Sensibilità wake word
Il select `WakeWord - Sensibilita` permette di modificare il cutoff del modello `okay_nabu`.
| Opzione | Effetto |
| ------- | ------- |
| Poco sensibile | cutoff più alto |
| Medio | cutoff intermedio |
| Molto sensibile | cutoff più basso |

---

## 🔁 Modalità conversazione
Lo switch `Assistente - Ascolto continuo` permette di scegliere tra:

* modalità continua
* modalità singola

Quando la modalità continua è attiva:
* il Voice Assistant viene avviato con `voice_assistant.start_continuous`
* un controllo periodico ferma il VA dopo inattività prolungata

Quando la modalità continua è disattivata:
* il Voice Assistant usa `voice_assistant.start`
* se il VA è in esecuzione, viene fermato quando si passa alla modalità singola

---

## 🎚️ Pulsanti fisici
I pulsanti fisici sono collegati al TCA9555.
| Pulsante | Pin esteso | Azione |
| -------- | ---------: | ------ |
| K1 | 9 | Volume - |
| K2 | 10 | Play/Pausa media player |
| K3 | 11 | Volume + |

Quando viene modificato il volume:
* viene mostrata la barra volume sul display
* il LED ring lampeggia in verde
* dopo un breve delay viene ripristinato lo stato LED corretto

---

## 🏠 Action ESPHome disponibili in Home Assistant
Il blocco `api.actions` espone alcune azioni richiamabili da Home Assistant.

### `set_led_color`
Imposta il colore base del LED ring.

| Parametro | Tipo | Descrizione |
| --------- | ---- | ----------- |
| `red` | float | Rosso 0.0 / 1.0 |
| `green` | float | Verde 0.0 / 1.0 |
| `blue` | float | Blu 0.0 / 1.0 |

### `start_va`
Avvia il Voice Assistant. Usa automaticamente:

* `voice_assistant.start_continuous` se la modalità continua è attiva
* `voice_assistant.start` se la modalità singola è attiva

### `stop_va`
Ferma il Voice Assistant.

### `set_alarm_time`
Imposta l'orario sveglia.

Formato richiesto:

```text
HH:MM
```

### `set_time_zone`
Imposta la timezone POSIX del dispositivo.

Esempio:

```text
CET-1CEST,M3.5.0,M10.5.0/3
```

---

## 🔔 Eventi inviati a Home Assistant
Il device può inviare eventi Home Assistant.
| Evento | Descrizione |
| ------ | ----------- |
| `esphome.alarm_ringing` | Sveglia scattata |
| `esphome.tts_uri` | URI del TTS generato |
| `esphome.stt_text` | Testo riconosciuto dallo STT |

---
## 🔄 Firmware check
Lo script `check_firmware_version` legge l'ultima versione disponibile dal file:
```text
OTA/version.yaml
```
Formato atteso:
```yaml
version: 5.1.0
```
Se la versione remota è diversa da quella locale:
* `fw_update_available` diventa `true`
* il display si sveglia
* viene mostrato un badge con la nuova versione firmware

Entità collegate:
| Entità | Tipo | Descrizione |
| ------ | ---- | ----------- |
| `Controllo nuovo firmware` | switch | Abilita/disabilita il controllo |
| `Controlla nuovo firmware` | button | Esegue il controllo manuale |
| `fw_latest_version` | global interna | Ultima versione rilevata |

---

## 🧪 Entità principali create in Home Assistant
Il nome completo dipende da `device_name` e `friendly_name`, ma le entità principali sono:

### Light
* `Anello LED`

### Media player
* `Media Player`

### Switch
* `Modalità antifurto`
* `Assistente - Ascolto continuo`
* `Amplificatore`
* `Microfono disattivato`
* `Suono Mute-Unmute`
* `Suono Wake Word`
* `Sveglia attiva`
* `Controllo nuovo firmware`

### Select
* `Display - Emozione`
* `WakeWord - Sensibilita`
* `Livello log`
* `Sveglia - Azione`

### Number
* `Soglia Rumore`
* `Mic-Guadagno`
* `VA-Guadagno`
* `Sopp. Rumore`
* `LED`

### Sensor
* `Rumore`
* `Prossimo timer`

### Text sensor
* `Prossimo timer - Nome`
* `Ora sveglia`
* `Ora dispositivo`

### Button
* `Test antifurto`
* `Riavvia`
* `Controlla nuovo firmware`

### Binary sensor
* `Rilevamento rumore`
* `K1 Volume -`
* `K2 Play-Pausa`
* `K3 Volume +`

---

## 🧰 Componenti esterni usati

Il progetto usa componenti esterni:
```yaml
external_components:
  - source: github://esphome/esphome@dev
    components:
      - micro_wake_word
    refresh: 0s

  - source:
      type: git
      url: https://github.com/sw3Dan/waveshare-s2-audio_esphome_voice
      ref: main
    components: [ es8311 ]
    refresh: 0s
```
Il componente `es8311` è usato per il DAC audio della board.

---

## ⚙️ Configurazione ESP32-S3
Il progetto usa:
* board `esp32-s3-devkitc-1`
* CPU a 240 MHz
* flash 16 MB
* PSRAM octal 80 MHz
* framework ESP-IDF
* ottimizzazioni cache e SPIRAM
* Bluetooth memory allocation su SPIRAM
* TLS 1.3 abilitato

---

## 🧯 Troubleshooting

### Il device non parte dopo il flash
* verifica alimentazione
* verifica cavo USB-C dati
* prova la modalità BOOT/RESET
* controlla i log ESPHome

### Il Voice Assistant non risponde
Controlla:

* API ESPHome connessa
* Voice Assistant connesso
* microfono non in mute
* pipeline Assist corretta in Home Assistant
* wake word avviata
* media player non in annuncio/musica

### La wake word non riparte
Lo script `ensure_wake_word_running` avvia MicroWakeWord solo se:
* API connessa
* Voice Assistant connesso
* microfono non in mute
* timer non in ringing
* Voice Assistant non già in esecuzione
* media player non in play
* media player non in announcement

### Il display resta acceso troppo spesso
Controlla:
* `Soglia Rumore`
* sensore `Rumore`
* eventuali rumori ambientali vicini al microfono
* musica/TTS/timer attivi

### L'antifurto scatta troppo facilmente
Alza la soglia `Soglia Rumore`.
Esempio:
* più sensibile: `-65 dB`
* medio: `-55 dB`
* meno sensibile: `-45 dB`

### Il microfono sente male
Regola:
* `Mic-Guadagno`
* `VA-Guadagno`
* `Sopp. Rumore`
* `WakeWord - Sensibilita`

### Il display va a scatti o la musica saltella
Possibili interventi:
* ridurre `update_interval` del display solo se necessario
* alleggerire la lambda del display
* aumentare `buffer_duration` dello speaker
* ridurre log verbose
* verificare alimentazione stabile
* evitare animazioni troppo pesanti durante playback audio
* se necessario, ridurre temporaneamente `data_rate` del display per testare stabilità SPI/alimentazione

---

## 🧭 Flusso operativo semplificato

1. Il device si avvia.
2. Si connette al Wi-Fi.
3. Si connette all'API ESPHome.
4. Inizializza audio, microfono, display e LED ring.
5. Avvia MicroWakeWord se le condizioni sono corrette.
6. Quando rileva la wake word:
   * riproduce il suono wake word, se abilitato
   * avvia il Voice Assistant
   * passa allo stato waiting/listening
7. Durante il comando:
   * aggiorna display e LED
   * applica ducking alla musica
   * intercetta eventuali local intent
8. Durante la risposta:
   * anima la bocca
   * mostra stato replying
   * gestisce TTS/announcement
9. Alla fine:
   * ripristina ducking
   * torna idle
   * riavvia la wake word
10. Se musica, timer, sveglia o antifurto sono attivi, la logica LED/display passa allo stato dedicato.

---

## 📌 Note finali
Questa configurazione è volutamente avanzata: non è solo un esempio base di Voice Assistant ESPHome, ma una piattaforma completa per sperimentare con:
* Home Assistant Assist
* wake word locale
* local intent direttamente su ESPHome
* audio I2S
* media player ESPHome
* display animati
* LED feedback
* timer
* sveglia
* antifurto locale
* tuning audio da dashboard
* firmware check da GitHub
È pensata per essere personalizzata, migliorata e adattata a diversi episodi e test del progetto **HackMyHome**.

---

## 🙏 Credits

Thanks to:
* **Sanji78** per il continuo supporto, test e verifica del codice
* **CaptainMustard** per il core code di partenza: [https://github.com/captainmustard/waveshare-s2-audio_esphome_voice](https://github.com/captainmustard/waveshare-s2-audio_esphome_voice)
* **sw3Dan** per il componente ES8311 e il lavoro sulla board Waveshare
* **Community ESPHome e Home Assistant**
