# Modbus connection Home Assistant > Growatt MOD ### TL3-XH
Met deze informatie is het mogelijk om een Growatt MOD ### TL3-XH omvormer te koppelen met Home Assistant via Modbus. Je dient hiervoor een USB > RS485 stick aan te schaffen. Bij de omvormer word een Modbus stekker geleverd, deze heb je nodig inclusief een kabel welke loopt van je omvormer naar je Home Assistant installatie.

## Benodigdheden (getest met)
- Home Assistant 2024.5.3
- USB naar RS485 converter (https://www.amazon.nl/dp/B0B87D9LNC/ref=pe_28126711_487805961_TE_item)
- Modbus stekker (geleverd bij de omvormer)
- Kabel tussen omvormer en Home Assistant installatie (ik heb UTP kabel gebruikt met harde kern)

## Kabel maken
Maak eerst een kabel tussen je converter en de Modbus stekker.
USB GND naar Modbus 15
USB A+ naar Modbus 3
USB B- naar Modbus 4

## Home Assistant instellen
1. Sluit de USB aan op een vrije USB poort van je Home Assistant installatie
2. Herstart Home Assistant
3. Ga naar Instellingen >  Systeem > Hardware > Alle hardware
4. Zoek naar de USB Converter (was bij mij ttyACM0) en kopieer het apparaatpad (bij mij /dev/ttyACM0)
5. Open een tekstbewerker in Home Assistant en ga naar de configuration.yaml
6. Voeg de volgende code in
