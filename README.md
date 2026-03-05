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
