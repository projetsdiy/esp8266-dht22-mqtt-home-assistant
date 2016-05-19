# ESP8266 + DHT22 + MQTT + Home-Assistant 
Un petit projet pour apprendre à fabriquer une sonde de température connectée (IoT).
Vous pouvez retrouver l'intégralité de ce projet sur mon blog
http://www.projetsdiy.fr/esp8266-dht22-mqtt-projet-objet-connecte/


<h2>Matériel nécessaire</h2>
Pour réaliser ce projet vous aurez besoin des éléments suivants :
- Un module WiFi ESP8266 de préférence basé sur un ESP-12. J'utilise une <a href="http://www.banggood.com/search/esp8266-nodemcu/0-0-0-1-3-45-0-price-0-0_p-1.html?sortType=asc?p=RA18043558422201601Y" target="_blank" rel="nofollow" data-mce-href="http://www.banggood.com/search/esp8266-nodemcu/0-0-0-1-3-45-0-price-0-0_p-1.html?sortType=asc?p=RA18043558422201601Y">Wemos D1 mini</a> qui coûte 5€ environ.
- Une sonde de température DHT11 ou <a href="http://geni.us/34qi" target="_blank" rel="nofollow" data-mce-href="http://geni.us/34qi">DHT22</a>
- Une Led
- Une résistance 220 ohms
- Un câble USB (pour programmer et alimenter l'ESP)
- Une batterie LiPo 3.7V ou un boitier de 2 piles AA si vous voulez 

<h2>Logiciels nécessaires</h2>
Pour recevoir les mesures de votre objets et piloter la led, vous aurez besoin d'un ordinateur avec les logiciels suivants d'installés. Un <a href="http://amzn.to/24FM8i6" target="_blank" rel="nofollow" data-mce-href="http://amzn.to/24FM8i6">Raspberry Pi 3</a> sera parfait
- Broker MQTT. Mosquitto est très simple et performant. Voici un article de présentation http://www.projetsdiy.fr/mosquitto-broker-mqtt-raspberry-pi/
- Un serveur domotique. Si vous débutez, Home-Assistant est très simple à installer et à utiliser même si l'anglais n'est pas votre fort (http://www.projetsdiy.fr/home-assistant-serveur-domotique-raspberry-pi/). 

<h2>Branchement</h2>
<img align="center" src="https://github.com/projetsdiy/esp8266-dht22-mqtt-home-assistant/blob/master/branchement%20esp8266%2Bled%2Bdht22.jpg" style="max-width:100%;">

<h2>Code</h2>
Téléchargez et ouvrez le fichier DTH22_LED.ino avec l'IDE Arduino
Si vous découvrez les modules ESP8266, lisez cet article qui explique comment les programmer avec l'IDE Arduino http://www.projetsdiy.fr/esp-01-esp8266-flasher-firmware-origine/

<h2>Installer Home-Assistant</h2>
Python 3 (ou supérieur) doit être présent sur votre ordinateur
puis 
<pre>pip3 install homeassistant</pre>

Pour les utilisateurs de Windows 10 (ou 7)
<pre>python -m pip install homeassistant</pre>

<h2>Intégration MQTT + Home-Assistant</h2>
Allez dans le répertroire d'installation d'Home-Assistant
<pre>cd ~/.homeassistant</pre>
puis 
<pre>sudo nano configuration.yaml </pre>
Ajoutez une section mqtt
<pre>
mqtt:
  broker: localhost          
  port: 1883                 #par défaut
  client_id: home-assistant-1
  keepalive: 60
  username: USERNAME  #optionnel
  password: PASSWORD   #optionnel
  protocol: 3.1              #par défaut
</pre>  

et maintenant deux sensors (température et humidité)
<pre>
sensor:
  platform: mqtt
  state_topic: "sensor/temperature"
  name: "Température"
  qos: 0
  unit_of_measurement: "°C"
  #value_template: '{{ payload }}'

sensor 2:
  platform: mqtt
  state_topic: "sensor/humidity"
  name: "Humidité"
  qos: 0
  unit_of_measurement: "°C"
  #value_template: '{{ payload }}'
</pre>  
et enfin un switch pour piloter la Led
<pre>
switch:
  platform: mqtt
  name: "Cuisine"
  command_topic: "homeassistant/switch1" #Topic sur lequel on publie l'état de l'interrupteur
  payload_on: "ON"      # ON pour allumer
  payload_off: "OFF"    # et OFF pour éteindre (à vous de choisir)
  optimistic: true      
  qos: 0
</pre>  
<br>
Rechargez la page d'Home-Assistant. Vous avez maintenant deux indicateurs (température et humidité) ainsi qu'un switch pour allumer / éteindre la Led

<img align="center" src="https://github.com/projetsdiy/esp8266-dht22-mqtt-home-assistant/blob/master/mqtt+home-assistant.png" style="max-width:100%;">

<bre>
J'espère que ce petit projet vous sera utile pour débuter en domotique. 
