# Modbus connection Home Assistant > Growatt MOD ### TL3-XH
Met deze informatie is het mogelijk om een Growatt MOD ### TL3-XH omvormer te koppelen met Home Assistant via Modbus. Je kan hiermee alle sensoren (nagenoeg realtime) uitlezen, maar ook de omvormer bij negatieve tarieven bijvoorbeeld uitschakelen. Je dient hiervoor een USB > RS485 stick aan te schaffen. Bij de omvormer word een Modbus stekker geleverd, deze heb je nodig inclusief een kabel welke loopt van je omvormer naar je Home Assistant installatie.

## Benodigdheden (getest met)
- Home Assistant 2024.5.3
- USB naar RS485 converter (https://www.amazon.nl/dp/B0B87D9LNC/ref=pe_28126711_487805961_TE_item)
- Modbus stekker (geleverd bij de omvormer)
- Kabel tussen omvormer en Home Assistant installatie (ik heb UTP kabel gebruikt met harde kern)

## Kabel maken
Maak eerst een kabel tussen je converter en de Modbus stekker.
- USB GND naar Modbus 15
- USB A+ naar Modbus 3
- USB B- naar Modbus 4

## Home Assistant instellen
1. Sluit de USB aan op een vrije USB poort van je Home Assistant installatie
2. Herstart Home Assistant
3. Ga naar Instellingen >  Systeem > Hardware > Alle hardware
4. Zoek naar de USB Converter (was bij mij ttyACM2) en kopieer het apparaatpad (bij mij /dev/ttyACM2)
5. Open een tekstbewerker in Home Assistant en ga naar de configuration.yaml
6. Voeg de volgende code in:

```
modbus:
  - name: modbus_hub
    type: serial
    port: /dev/ttyACM2
    baudrate: 9600
    bytesize: 8
    method: rtu
    parity: N
    stopbits: 1
    sensors:
      - name: "custom_modbus_power_heigh"
        input_type: input
        address: 1
        scan_interval: 10
        slave: 1
      - name: "custom_modbus_energy_today"
        input_type: holding
        address: 5
        scan_interval: 10
        slave: 1
```

_De sensoren zijn puur ter illustratie, je kan hier zelf invoeren wat je wil. Je kan deze info vinden in de Modbus documentatie van Growatt (https://www.amosplanet.org/modbus-protocol-type-for-grid-hybrid-inverter/). Echter voor het in- en uitschakelen van de omvormer is dit niet van toepassing._

7. Herstart home assistant
8. Je kan nu communiceren met je Growatt omvormer.
9. Met deze YAML code kan je op de dashboard knoppen maken om in- of uit te schakelen:
![image](https://github.com/martijntenpas/growatt-tl3-xh/assets/84871327/2e324d04-f251-4f96-9671-097643c4989f)
```
type: grid
cards:
  - show_name: true
    show_icon: true
    type: button
    tap_action:
      action: call-service
      service: modbus.write_register
      target: {}
      data:
        hub: modbus_hub
        address: 0
        slave: 1
        value: 1
    entity: sensor.custom_modbus_energy_today
    name: Growatt on
  - show_name: true
    show_icon: true
    type: button
    tap_action:
      action: call-service
      service: modbus.write_register
      target: {}
      data:
        hub: modbus_hub
        address: 0
        slave: 1
        value: 0
    entity: sensor.custom_modbus_energy_today
    name: Growatt Off
```

Voor het inschakelen kan je ook een automatisering gebruiken
1. Actie > Service Aanroepen > Schrijfregeister
2. Adres is 0
3. Slave aan, is 1
4. Waarde 1 is aan, 0 is uit
5. Hub is modbus_hub
![image](https://github.com/martijntenpas/growatt-tl3-xh/assets/84871327/46dbf34d-0dc3-4ae0-98ee-ccb7139d5b87)


