# Domoticz-SmartStuff-P1
DzVents(Lua) Script for connecting Smart-Stuff P1 port (not Pro) Wifi dongle to Domoticz.

This is an older type P1 dongle based on an Esp8266, constructed and programmed by Martijn Hendriks / Smart-Stuff (https://github.com/mhendriks).

Originally there was a setup for Domoticz on his website but this is no longer the case.

This script is an adaptation from an already excisting script to be used with dzVents of Domoticz.

Used firmware on the Dongle : DSMR-API 3.5.9+1284 (13/08/2023) 

<b>Setup :</b>
  * The dongle setup can be done by connecting directly (with a smart phone etc) to its Wifi - IP adres.
  * Configure wifi for your network, save these. Be patient for the device to reboot and connect to your wifi network.
  * The device creates every second at <code>api/v2/sm/actual</code> a .json file from the received DSMR meter P1 telegram.
  * Change in the script the local DSMR_IP adres used for your needs (here 192.168.2.102)
  * <b>JSON.lua</b> must be in your <code>domoticz/scripts/lua</code>. If not present, get it here : https://regex.info/blog/lua/json
  * When used with a Docker setup of Domoticz change the Docker location of your scripts/lua (?)
  * The .json files are imported into the script and decoded using JSON.lua
  * Create 2 virtual P1 meter devices ; one for electricity (here 2303) and one for Gas (here 2304)
  * Copy, paste, save and activate the script in dzVents.

<b>Debugging :</b>
Use RESTer (or alike) in your browser to connect to the dongle's API.

<b>Known issues :</b>
  * Power : the P1 port on my ZIV (Enexis) meter doesn't deliver enough power for the dongle. An external 5 volts poweradapter is needed.
  * The dongle works well for the task but is slow in responding to setup and connecting etc
  * strange things happen when the script is saved without change, changing someting (a space) helps...(?)
