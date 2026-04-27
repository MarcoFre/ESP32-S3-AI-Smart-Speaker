### Aggiunto

* Local intent su testo STT per:
  * volume su
  * volume giù
  * stop locale del Voice Assistant tramite “modalità silenziosa”
  * attivazione antifurto
  * disattivazione antifurto
* Nuove substitution per:
  * mappa stati Voice Assistant
  * pin display GC9A01A
  * pin I2C/I2S
  * LED ring
  * TCA9555 extended IO
  * selezione canale microfono
  * tuning audio I2S
  * parole chiave local intent
* Display GC9A01A con:
  * music mode reattiva all'audio
  * VU meter laterale
  * barra volume laterale
  * sleep automatico
  * wake da rumore ambientale
  * badge aggiornamento firmware
  * modalità antifurto/startled
* Sound level meter basato su `playback_peak`.
* Modalità antifurto locale con soglia configurabile.
* Sveglia configurabile da Home Assistant.
* Firmware check automatico e manuale.
* Action ESPHome:
  * `set_led_color`
  * `start_va`
  * `stop_va`
  * `set_alarm_time`
  * `set_time_zone`

### Modificato

* Aggiornata documentazione README alla versione `5.1.0`.
* Aggiornata descrizione della state machine.
* Aggiornata sezione pinout.
* Aggiornata sezione troubleshooting.
* Aggiornata configurazione display: `data_rate: 80MHz`.
* Migliorata descrizione della gestione wake word dopo media, annunci, timer e Voice Assistant.

