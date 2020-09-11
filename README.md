# Home Assistant 
**Centrum Dowodzenia MądregoDomu**

TO DO:
- ukończyć pisanie README
- podzielić na podstrony README (o ile się da - zbadać temat)
- zaktualizować dane

1. Historia
2. Pod maską
3. 

---
## Krótka historia

Swoją przygodę z Inteligentnym domem zacząłem na przełomie 2018 i 2019 roku. Od początku wiedziałem, że chcę aby mój system działał lokalnie, bez wysyłania informacji do przyjaciół z Chin. Google i Apple wiedzą już wystarczajaco dużo ...
Przygodę rozpocząłem od Domoticza postawionego na Raspberry Pi B+, które posiadałem od 2013 roku do zabaw z kopaniem BTC. Szybko okazało się, że sprzęt ten jest już wolny i trochę zawodny. Nadaszła pora na zmianę w styczniu 2019 roku. Domoticz wylądował na Raspberry Pi 3B+ z 32GB kartą SD. 
Wówczas już, po rozeznaniu się w istniejących technologiach, podjąłem decyzje, że mój MądryDom (MD) będzie funkcjonować z urządzeniami Wi-Fi, RF oraz technologią Z-Wave, która wówczas wydawała mi się o niebo lepsza od ZigBee.
Zakupiłem kilka Sonoff'ów Basic oraz gniazdek S20, i rozpocząłęm swoje testy. Na pierwszy ogień poszedł alternatywy firmware wspomnianych Sonoff'ów. Z Holandi, tfu! z Niderlandów, przyszedł moduł RF oparty na Andruino oraz na pokładzie zagościł Aeotec Z-Stick Gen5 - moduł Z-Wave USB, zaś po okazyjnej cenie z Chin przypłynęły gniazdka Neo Coolcam NAS-WR01ZE Z-wave Plus Smart Power Plug EU Socket. Niestety od razu były z nimi problemy, a współpraca z Domoticzem prawie nie możliwa. Rozpocząłem poszukiwania nowych rozwiązań.

W marcu 2019 po raz pierwszy Centrum Dowodzenia MD spotkało Home Assistanta i zagościło na karcie SD w RPi 3B+. Jak prawie każdy z użytkowników wie, karty SD w RPi są "lekko kłopotliwe" z punktu widzenia niezawodności przy bazach danych (i dużych ilościach ruchu n karcie). Tak nie mogło być dalej skoro chciałem, aby system był niezawodny! 

> ___ciekawostka:___
>w ciągu 4 miesięcy uszkodziło się 7 kart SD (Samsung, Kingstone, Sandisk), o pojemności od 8 do 32GB. 

Nastąpiły pirwsze większe przenosiny. Home Assistant v0.9 wylądował na dysku SSD bootowalnym z RPi3B+. Ta prędkośc! ta niezawodność! byłem zachwycony!
Równocześnie ze zmianami w sercu systemu, cały ekosystem domu rozwijał się. Doszły kolejne urządzenia z-wave (czujnik dymy, czujnik gazu, kontaktron), rozwinęła się baza elementów opartych na chipie ESP8266 oraz kilka czujników RF. Kolejno doszły także kamery IP :man_facepalming: po wi-fi :man_facepalming: ale także zapisujące na własnych kartach SD.
Zbudowałem wówczas także swój pierwszy Smogomierz... to były piękne czasy :)

>___ciekawostka:___
>w kamerach znajdują się chińskie karty SD marki "Kingstone" o poj 64GB. Działają - odpukać - niezawodnie od marca/kwietnia 2019 r.

Kolejne zmiany rozpoczęły się od słabej wydajności ówczasnych rozwiązań sieciowych w domu. Po przewertowaniu internetów i youtubów wybór mógł być tylko jeden - Ubiquiti, a dokładniej UniFi. Po przesiadce Wszystkie urządzenia sieciowe zyskały nowe życie, jak również bezpieczeństwo poszło w górę. UniFi Controllera nie nabyłem, więc postanowiłem rozpoznać czym są te "dockery", natomiast szerokopojęte IoT wylądowało na osobnym SSID z odciętym dostępem do internetu (oczywiście z wyjątkami TV, nBoxy itp.). Obok UniFi Controllera w dockerze wylądował także Home Assistant, Node-red i wszystkie inne elementy znajdujące się na SSD-ku. 

Być może przenosiny do "kontenerów", a być może także wysokie wakacyjne temperatury powodowały, że RPi potrafiło się rozgrzać i osiągać nawet 80st! (Na chipach RPi zamontowane były radiatorki, zaś na przestrzałchłodziły je dwa małe wiatraczki). Temperatura i zapewne obciążenie powodowały, że RPi stawało się zawodne i co chwile wieszało.

Nadszedł czas na NAS :) 
We wrześniu 2019AD zdecydowałem się na zakup QNap'a TS-251B-4GB oraz 2 dysków WD Red o pojemności 1TB. Od razu okazało się, że 4GB to o dużo za mało i zwiększyłem pamięć RAM do 16GB. Niestety nie przemyślałem opcji dysków. Teraz już wiem, że 1TB w RAID to o dużo za mało, ale mam przynajmniej co planować.... Obraz z kamer IP już nie tylko ląduje na kartach SD, ale jest też zapisywany jest na serwerze plików. 

Home Assistant trafił na NASa. Poczatkowo w dockerze. Był to błąd! Ciągłe restarty NASa, Zaweiszanie się HA. Dramat i "myślenice" czy dobrze zrobiłem przesiadając się z RPi. Odłączałem rózne serwisy, nic nie pomagało, a maksymalny uptime nie przekraczał 24h. W chwili desperacji zrezygnowałem z UniFi Controllera w dockerze i zakupiłem Unifi CloudKey.
Po rozmowach ze znajomymi, supportem QNapa i przewertowaniu for internetowych zmieniłem strategię. Porzuciłem dockera na rzecz maszyny wirtualnej. Zmiana inplus była natychmiastowa! Choć nadal występowały problemy, jednak czas ciągłego działąnia dochodził do 5 - 6 dni, a problemy po restarcie ograniczały się tylko do sprawdzenia systemu plików, wcześniej niejednokrotnie musiałem przywracać cały firmware NASa i od nowa tworzyć dockera HomeAssistanta ....

>___ciekawostka:___
>problem niestabilnej pracy QNap'a rozwiązał ostatni update firmware o oznaczeniu 4.4.1.1146 z dnia 06.12.2019r. Od tego czasu system działa stabilnie bez restartów.





---

## Ekosystem

**Internet :arrow_down_small:60Mbps / :arrow_up_small:10Mbps**

- :large_blue_diamond:[UniFi USG](https://www.ui.com/unifi-routing/usg/ "UniFi USG")
- :large_blue_diamond:[UniFi US-8](https://www.ui.com/unifi-switching/unifi-switch-8/ "UniFi US-8")
 - :key:[UniFi UC-CK](https://www.ui.com/unifi/unifi-cloud-key/ "UniFi UC-CK")
 - :tv:[Samsung SmartTV 40H6400](https://www.samsung.com/uk/tvs/full-hd-h6400/UE40H6400AKXXU/ "Samsung SmartTV 40H6400")    
 - :small_blue_diamond:[QNap TS-251B RAM: 16GB  HDD: 2x1TB WD Red](https://www.qnap.com/pl-pl/product/ts-251b "QNap TS-251B RAM: 16GB  HDD: 2x1TB WD Red")
     - :twisted_rightwards_arrows:[Aeotec Z-Stick Gen5](https://aeotec.com/z-wave-usb-stick/ "Aeotec Z-Stick Gen5")
         - :electric_plug:[Neo Coolcam NAS-WR01ZE](https://pl.aliexpress.com/item/32787926055.html "Neo Coolcam NAS-WR01ZE") - Salon
         - :electric_plug:[Neo Coolcam NAS-WR01ZE](https://pl.aliexpress.com/item/32787926055.html "Neo Coolcam NAS-WR01ZE") - Livingroom
         - :electric_plug:[Neo Coolcam NAS-WR01ZE](https://pl.aliexpress.com/item/32787926055.html "Neo Coolcam NAS-WR01ZE") - Livingroom
         - :electric_plug:[Neo Coolcam NAS-WR01ZE](https://pl.aliexpress.com/item/32787926055.html "Neo Coolcam NAS-WR01ZE") - Kitchen
         - :door:[Sensative Strips](https://sensative.com/strips/ "Sensative Strips") - Doors
         - :cloud:[Heiman Combustible Gas Sensor HS1CG-Z](http://www.heimantech.com/product/76.html "Heiman Combustible Gas Sensor HS1CG-Z") - Kitchen
         - :fire:[devolo Home Control Smoke Detector](https://www.devolo.co.uk/devolo-home-control-smoke-detector "devolo Home Control Smoke Detector") - Salon
     - :twisted_rightwards_arrows:[Sonoff RF Bridge](https://sonoff.tech/product/accessories/433-rf-bridge "Sonoff RF Bridge") with [Tasmota](https://github.com/arendst/Tasmota "Tasmota")
         - :ocean:[RF Flood Sensor](https://www.houseiq.pl/pl/p/Czujnik-zalania-z-antena-RF-433-MHz-Daleki-zasieg/776 "RF Flood Sensor") - Basement
         - :radio_button:[RF Button](https://www.houseiq.pl/pl/p/Pilot-1-kanalowy-RF433MHz-Przycisk-dzwonek/826 "RF Button") - Kitchen
         - :radio_button:[RF Button](https://www.houseiq.pl/pl/p/Pilot-1-kanalowy-RF433MHz-Przycisk-dzwonek/826 "RF Button") - Livingroom      
         - :bulb:RF LED Christmass Lights [DIY](https://pl.aliexpress.com/item/33008266249.html?spm=a2g0s.9042311.0.0.27425c0fTLgmi8 "DIY") - Porch
         - :bulb:RF LED Christmass Lights [DIY](https://pl.aliexpress.com/item/33008266249.html?spm=a2g0s.9042311.0.0.27425c0fTLgmi8 "DIY") - Tree 1
         - :bulb:RF LED Christmass Lights [DIY](https://pl.aliexpress.com/item/33008266249.html?spm=a2g0s.9042311.0.0.27425c0fTLgmi8 "DIY") - Tree 2
         - :bulb:RF LED Christmass Lights [DIY](https://pl.aliexpress.com/item/33008266249.html?spm=a2g0s.9042311.0.0.27425c0fTLgmi8 "DIY") - Cabinet Salon
         - :bulb:RF LED Christmass Lights [DIY](https://pl.aliexpress.com/item/33008266249.html?spm=a2g0s.9042311.0.0.27425c0fTLgmi8 "DIY") - Wardrobe Livingroom
 - :signal_strength:[UniFi UAP-AC-Lite](https://www.ui.com/unifi/unifi-ap-ac-lite/ "UniFi UAP-AC-Lite")
     - :electric_plug:[Sonoff S2x](https://sonoff.tech/product/wifi-smart-plugs/s20 "Sonoff S2x") with [Tasmota](https://github.com/arendst/Tasmota "Tasmota") - Porch
     - :electric_plug:[Sonoff S2x](https://sonoff.tech/product/wifi-smart-plugs/s20 "Sonoff S2x") with [Tasmota](https://github.com/arendst/Tasmota "Tasmota") - Salon/Christmass Tree
         - :sunny:[DHT11](https://www.banggood.com/3Pcs-KY-015-DHT11-Temperature-Humidity-Sensor-Module-For-Arduino-p-983260.html?rmmds=search&cur_warehouse=CN "DHT11")
     - :electric_plug:[Sonoff S2x](https://sonoff.tech/product/wifi-smart-plugs/s20 "Sonoff S2x") with [Tasmota](https://github.com/arendst/Tasmota "Tasmota") - Livingroom
     - :electric_plug:[Sonoff S2x](https://sonoff.tech/product/wifi-smart-plugs/s20 "Sonoff S2x") with [Tasmota](https://github.com/arendst/Tasmota "Tasmota") - Basement
         - :sunny:[DHT11](https://www.banggood.com/3Pcs-KY-015-DHT11-Temperature-Humidity-Sensor-Module-For-Arduino-p-983260.html?rmmds=search&cur_warehouse=CN "DHT11")
     - :zap:[Sonoff Basic](https://sonoff.tech/product/wifi-diy-smart-switches/basicr2 "Sonoff Basic") LED Strips with [Tasmota](https://github.com/arendst/Tasmota "Tasmota") - Basement
     - :zap:[Sonoff Basic](https://sonoff.tech/product/wifi-diy-smart-switches/basicr2 "Sonoff Basic") WaterPump with [Tasmota](https://github.com/arendst/Tasmota "Tasmota") - Basement
     - :bulb:[AriLux LC01](https://www.banggood.com/ARILUX-AL-LC01-Super-Mini-LED-WIFI-Smart-RGB-Controller-For-RGB-LED-Strip-Light-DC-9-12V-p-1058603.html?cur_warehouse=CN "AriLux LC01") RGB LED Strip with [Tasmota](https://github.com/arendst/Tasmota "Tasmota") - Kitchen
     - :bulb:[AriLux LC01](https://www.banggood.com/ARILUX-AL-LC01-Super-Mini-LED-WIFI-Smart-RGB-Controller-For-RGB-LED-Strip-Light-DC-9-12V-p-1058603.html?cur_warehouse=CN "AriLux LC01") RGB LED Strip with [Tasmota](https://github.com/arendst/Tasmota "Tasmota") - Livingroom
     - :cloud:DIY Dustmeter (smogomierz)
         - :floppy_disk:[Wemos D1 Pro Mini](https://www.banggood.com/Geekcreit-D1-Mini-Pro-16-Module-ESP8266-Series-WiFi-Wireless-Antenna-p-1144951.html?rmmds=search&cur_warehouse=CN "Wemos D1 Pro Mini") [Tasmota](https://github.com/arendst/Tasmota "Tasmota") - Porch 
             - :cloud:[SDS011](https://www.banggood.com/Geekcreit-Nova-PM-Sensor-SDS011-High-Precision-Laser-PM2_5-Air-Quality-Detection-Sensor-Module-Tester-p-1144246.html?rmmds=search&cur_warehouse=CN "SDS011") - Porch
             - :sunny:[BMP280](https://www.banggood.com/GY-21P-Atmospheric-Humidity-Temperature-Sensor-Barometric-Pressure-BMP280-SI7021-p-1200466.html?rmmds=search&cur_warehouse=CN "BMP280") - Porch
             - :sunny:[DHT11](https://www.banggood.com/3Pcs-KY-015-DHT11-Temperature-Humidity-Sensor-Module-For-Arduino-p-983260.html?rmmds=search&cur_warehouse=CN "DHT11") - Attic
             - :door:[Magnetic Sensor](https://www.banggood.com/Door-Or-Window-Contact-Magnetic-Reed-Switch-Alarm-p-91677.html?rmmds=search&cur_warehouse=CN "Magnetic Sensor") - Attic
     - :floppy_disk:[Raspberry Pi Zero W](https://botland.com.pl/pl/moduly-i-zestawy-raspberry-pi-zero/8743-zestaw-raspberry-pi-zero-w-all-in-one-5903351240116.html "Raspberry Pi Zero W") - Livingroom
         - :sunflower:[Xiaomi miFlora](https://www.aliexpress.com/i/32949499909.html "Xiaomi miFlora")
         - :sunflower:[Xiaomi miFlora](https://www.aliexpress.com/i/32949499909.html "Xiaomi miFlora")
         - :sunflower:[Xiaomi miFlora](https://www.aliexpress.com/i/32949499909.html "Xiaomi miFlora")
         - :sunflower:[Xiaomi miFlora](https://www.aliexpress.com/i/32949499909.html "Xiaomi miFlora")
         - :sunflower:[Xiaomi miFlora](https://www.aliexpress.com/i/32949499909.html "Xiaomi miFlora")
         - :sunflower:[Xiaomi miFlora](https://www.aliexpress.com/i/32949499909.html "Xiaomi miFlora")
         - :sunflower:[Xiaomi miFlora](https://www.aliexpress.com/i/32949499909.html "Xiaomi miFlora")
     - :floppy_disk:[Raspberry Pi Zero W](https://botland.com.pl/pl/moduly-i-zestawy-raspberry-pi-zero/8743-zestaw-raspberry-pi-zero-w-all-in-one-5903351240116.html "Raspberry Pi Zero W") - Porch
         - :sunflower:[Xiaomi miFlora](https://www.aliexpress.com/i/32949499909.html "Xiaomi miFlora")
         - :sunflower:[Xiaomi miFlora](https://www.aliexpress.com/i/32949499909.html "Xiaomi miFlora")
         - :sunflower:[Xiaomi miFlora](https://www.aliexpress.com/i/32949499909.html "Xiaomi miFlora")
         - :sunflower:[Xiaomi miFlora](https://www.aliexpress.com/i/32949499909.html "Xiaomi miFlora")
         - :sunflower:[Xiaomi miFlora](https://www.aliexpress.com/i/32949499909.html "Xiaomi miFlora")
         - :sunflower:[Xiaomi miFlora](https://www.aliexpress.com/i/32949499909.html "Xiaomi miFlora")
         - :sunflower:[Xiaomi miFlora](https://www.aliexpress.com/i/32949499909.html "Xiaomi miFlora")     
     - :sunny:[Netatmo Weather Station](https://www.netatmo.com/pl-pl/weather/weatherstation "Netatmo Weather Station")
         - :sunny:[Main Indoor Module](https://shop.netatmo.com/pln_pl/weatherstation.html "Main Indoor Module") - Livingroom
         - :sunny:[Additional Indoor Module](https://shop.netatmo.com/pln_pl/additional-indoor-module.html "Additional Indor Module") - Salon
         - :sunny:[Main Outdoor Module](https://shop.netatmo.com/pln_pl/weatherstation.html "Main Outdoor Module") - Porch
         - :sunny:[Additional Outdoor Module](https://shop.netatmo.com/pln_pl/netatmo-outdoor-module.html "Additional Outdoor Module") - Garden
         - :umbrella:[Rain Gauge](https://shop.netatmo.com/pln_pl/rain-gauge.html "Rain Gauge") - Garden
         - :flags:[Anemometr](https://shop.netatmo.com/pln_pl/wind-gauge.html "Anemometr") - Garden
     - :satellite:[nBox ADB Enigma2](https://allegro.pl/oferta/nbox-bzzb-250gb-karta-dekoder-nc-enigma2-e2-6886073915 "nBox ADB Enigma2") - Livingroom
     - :satellite:[nBox ADB Enigma2](https://allegro.pl/oferta/nbox-bzzb-250gb-karta-dekoder-nc-enigma2-e2-6886073915 "nBox ADB Enigma2") - Salon 
     - :movie_camera:[JIENU IPcamera](https://pl.aliexpress.com/item/32832214354.html?spm=a2g0s.9042311.0.0.27425c0fC4otI4 "JIENU IP Camera") - Porch
     - :movie_camera:[JIENU IPcamera](https://pl.aliexpress.com/item/32832214354.html?spm=a2g0s.9042311.0.0.27425c0fC4otI4 "JIENU IP Camera") - Garden
     - :bulb:[Xiaomi Mijia Philips Bulb E27](https://pl.aliexpress.com/item/32839562830.html?spm=a2g0s.9042311.0.0.7f865c0fx7z0x4 "Xiaomi Mijia Philips Bulb E27") - Livingroom - do zintegrowania
     - :bulb:[Xiaomi Mijia Philips Bulb E27](https://pl.aliexpress.com/item/32839562830.html?spm=a2g0s.9042311.0.0.7f865c0fx7z0x4 "Xiaomi Mijia Philips Bulb E27") - Livingroom - do zintegrowania
     - :bulb:[Xiaomi Mijia Philips Bulb E27](https://pl.aliexpress.com/item/32839562830.html?spm=a2g0s.9042311.0.0.7f865c0fx7z0x4 "Xiaomi Mijia Philips Bulb E27") - Livingroom - do zintegrowania
     - :iphone:[Apple Iphone 8](https://www.apple.com/pl/shop/buy-iphone/iphone-8 "Apple Iphone 8")
     - :iphone:[Apple Iphone 11](https://www.apple.com/pl/iphone-11/ "Apple Iphone 11")
     - :watch:[Apple Watch Series 5](https://www.apple.com/pl/apple-watch-series-5/ "Apple Watch Series 5")
     - :computer:[DELL Inspirion 5482 i7 16gb 256SSD](https://www.dell.com/pl-pl/shop/cty/pdp/spd/inspiron-15-5593-laptop/cn59307 "DELL Inspirion 5482")
     - :computer:[ASUS Transformer Book T100HA](https://www.asus.com/pl/2-in-1-PCs/ASUS_Transformer_Book_T100HA/ "ASUS transformer Book T100HA")
     - :computer:[nVidia Shield K1](https://www.x-kom.pl/p/268648-tablet-8-nvidia-shield-tablet-k1.html "nVidia Shield K1")
     - :dog:[Tractive for Dogs](https://tractive.com/en/pd/gps-tracker-dog "Tractive for Dogs") - :dog: Frodo :dog: - do zintegrowania
     - :cat:[Tractive for Cats](https://tractive.com/en/pd/gps-tracker-cat "Tractive for Cats") - :cat: Inspektor :cat: - do zintegrowania
  



## Home Assistant

Obecnie pracuje system w wersji [Home Assistanta 0.104.0](https://www.home-assistant.io/ "Home Assistant")

### Home Asistant AddOns
- 🔹  [Configurator](https://github.com/home-assistant/hassio-addons/tree/master/configurator "Configurator")     v. 4.2
        Browser-based configuration file editor for Home Assistant
 
- 🔹  [ESPHome](https://esphome.io/ "ESPHome")                                                                    v. 1.13.6
        ESPHome Hass.io add-on for intelligently managing all your ESP8266/ESP32 devices.
 
- 🔹  [Log Viewer](https://github.com/hassio-addons/addon-log-viewer "Log Viewer")                                v. 0.6.4
        Browser-based log utility for Hass.io
 
- 🔹  [Mosquitto broker](https://www.home-assistant.io/addons/mosquitto/ "MQTT broker")                           v. 5.1
        An Open Source MQTT broker
 
- 🔹  [Node-RED](https://github.com/hassio-addons/addon-node-red "Node-RED")                                      v. 6.0.2
        Flow-based programming for the Internet of Things
 
- 🔹  [Samba share](https://www.home-assistant.io/addons/samba/ "Samba")                                          v. 9.0
        Expose Hass.io folders with SMB/CIFS
 
- 🔹  [TasmoAdmin](https://github.com/hassio-addons/addon-tasmoadmin "TasmoAdmin")                                v. 0.8.4
        Centrally manage all your Sonoff-Tasmota devices
 
- 🔹  [Visual Studio Code](https://github.com/hassio-addons/addon-vscode "Visual Studio Code")                    v. 1.2.2
        Fully featured VSCode experience, to edit your HA config in the browser, including auto-completion!

---
### Home Assistant Comunity Store - *[HACS](https://github.com/hacs/integration "HACS")*                                                                   v. 0.20.8
Manage (Install, track, upgrade) and discover custom elements for Home Assistant.

#### Integrations
- 🔹  [Antistorm sensor](https://github.com/PiotrMachowski/Home-Assistant-custom-components-Antistorm "Antistorm Sensor")                   v. 1.0.0
        This sensor uses official API to get storm warnings from [https://antistorm.eu](https://antistorm.eu "https://antistorm.eu").

- 🔹  [Attributes](https://github.com/pilotak/homeassistant-attributes "Attributes")                                                        v. 1.0.0
        Breaks out specified attribute from other entities to a sensor

- 🔹  [Auto Backup](https://github.com/jcwillox/hass-auto-backup "Auto Backup")                                                             v. 0.4.1
        Improved Backup Service for Hass.io that can Automatically Remove Snapshots and Supports Generational Backup Schemes.

- 🔹  [Average sensor](https://github.com/Limych/ha-average "Average sensor")                                                               v. bb1cdba
        Average Sensor for Home Assistant

- 🔹  [Breaking Changes](https://github.com/custom-components/breaking_changes "Breaking Changes")                                          v. 0.3.6
        Component to show potential breaking_changes in the current published version based on your loaded components

- 🔹  [Burze.dzis.net sensor](https://github.com/PiotrMachowski/Home-Assistant-custom-components-Burze.dzis.net "Burze.dzis.net sensor")    v. 1.0.0
        This sensor uses official API to get weather warnings for Poland and storm warnings for Europe from [https://burze.dzis.net](https://burze.dzis.net "https://burze.dzis.net")

- 🔹  [Looko2 sensor](https://github.com/PiotrMachowski/Home-Assistant-custom-components-Looko2 "Looko2 sensor")                            v. 1.0.0
        This sensor uses official API to get air quality data from [https://looko2.com](https://looko2.com "https://looko2.com").

- 🔹  [UniFi Gateway](https://github.com/custom-components/sensor.unifigateway "UniFi Gateway")                                             v. 0.2.3
        High level health status of UniFi Security Gateway devices via UniFi Controller

- 🔹  [Xiaomi BLE monitor sensor platform](https://github.com/custom-components/sensor.mitemp_bt "Xiaomi BLE monitor sensor platform")      v. 0.4.1
        Xiaomi BLE monitor sensor

- 🔹  [fontawesome](https://github.com/thomasloven/hass-fontawesome "fontawesome")                                                          v. 1.0
        Use icons from fontawesome in home-assistant

- 🔹  [iCloud3 Device Tracker](https://github.com/gcobb321/icloud3 "iCloud3 Device Tracker")                                                v. 2.0.5
        iCloud3 is a device_tracker custom_component for iPhones, iPads and iWatches. It is tightly integrated with the Home Assistant iOS Companion App (v1 & v2).


#### Plugins
- 🔹  [Bar Card](https://github.com/custom-cards/bar-card "Bar Card")                                                                        v. 1.6.2
        Customizable Animated Bar card for Home Assistant Lovelace

- 🔹  [Bignumber Card](https://github.com/custom-cards/bignumber-card "Bignumber Card")                                                      v. 0.0.2

- 🔹  [Button Card](https://github.com/custom-cards/button-card "Button Card")                                                               v. 3.1.1
        Lovelace button-card for home assistant

- 🔹  [Calendar Card](https://github.com/ljmerza/calendar-card "Calendar Card")                                                              v. 3.104.0
        Home Assistant Lovelace UI Custom Google Calendar Card

- 🔹  [Compact Custom Header](https://github.com/maykar/compact-custom-header "Compact Custom Header")                                       v. 1.5.0
        CCH - Customize the header and add enhancements to Lovelace. Features: kiosk mode, conditional header styling, per user/device views, swiping between views on mobile, and more.

- 🔹  [Flexible Horseshoe Card for Lovelace](https://github.com/AmoebeLabs/flex-horseshoe-card "Flexible Horseshoe Card for Lovelace")       v. 0.8.3
        Flexible Horseshoe card for Home Assistant Lovelace UI. A card with a flexible layout, a horseshoe-like donut graph, multiple entities or attributes, graphics and animations!

- 🔹  [Card Waze Travel Time](https://github.com/r-renato/ha-card-waze-travel-time "Card Waze Travel Time")                                  v. 48a87a5
        Home Assistant Lovelace card for Waze Travel Time Sensor

- 🔹  [Home Setter](https://github.com/custom-cards/home-setter "Home Setter")                                                               v. 0.0.2

- 🔹  [Logbook Card](https://github.com/royto/logbook-card "Logbook Card")                                                                   v. 0.4.2
        Logbook card for Home Assistant UI Lovelace

- 🔹  [Lovelace Swipe Navigation](https://github.com/maykar/lovelace-swipe-navigation "Lovelace Swipe Navigation")                           v. 1.2.2
        Swipe through Lovelace views on mobile.

- 🔹  [Miflora Card](https://github.com/RodBr/miflora-card "MiFlora Card")                                                                   v. 0.1.0
        A Home Assistant Lovelace card to report MiFlora plant sensors based on the HA Plant Card.

- 🔹  [Mini Graph Card](https://github.com/kalkih/mini-graph-card ""Mini Graph Card")                                                        v. 0.9.0-beta
        Minimalistic graph card for Home Assistant Lovelace UI

- 🔹  [Mini Media Player](https://github.com/kalkih/mini-media-player "Mini Media Player")                                                   v. 1.5.2
        Minimalistic media card for Home Assistant Lovelace UI

- 🔹  [RGB Light Card](https://github.com/bokub/rgb-light-card "RGB Light Card")                                                             v. 1.3.0
        A Lovelace custom card for RGB lights

- 🔹  [Unused Card](https://github.com/custom-cards/unused-card "Unused Card")                                                               v. 1.1
        All your unused entities in a list

- 🔹  [Weather Card](https://github.com/bramkragten/weather-card "Weather Card")                                                             v. 4.1
        Weather Card with animated icons for Home Assistant Lovelace

- 🔹  [auto-entities](https://github.com/thomasloven/lovelace-auto-entities "auto-entities")                                                 v. 14
        Automatically populate the entities-list of lovelace cards

- 🔹  [card-mod](https://github.com/thomasloven/lovelace-card-mod "card-mod")                                                                v. 12
        Add CSS styles to (almost) any lovelace card

- 🔹  [slider-entity-row](https://github.com/thomasloven/lovelace-slider-entity-row "slider-entity-row")                                     v. 10
        Add sliders to entity cards


#### Themes
- 🔹  [Clear Theme Dark](https://github.com/naofireblade/clear-theme-dark "Clear Theme Dark")                                                v. 1.1
        Dark variant of Clear Theme for Home Assistant

- 🔹  [Dark Teal](https://github.com/aFFekopp/dark_teal "Dark Teal")                                                                         v. 1.1
        Dark Theme based on clear-theme-dark by @naofireblade

- 🔹  [Slate](https://github.com/seangreen2/slate_theme "Slate")                                                                             v. 7f3e51d
        A Dark Theme for Home Assistant

- 🔹  :new::top:[iOS Dark Mode Theme](https://github.com/basnijholt/lovelace-ios-dark-mode-theme "iOS Dark Mode Theme")                      v. c9edd42
        Theme based on iOS Dark Mode for Lovelace Home Assistant


### Node-Red

