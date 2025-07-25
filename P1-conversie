-------------------------------------------------------------------------------------------
-- script_time_esp-dsmr-logger.lua     door: Michel Groen
-- aangepast voor : 
-- DSMR-API ver 1.3                    door: Henk Koorn
-- P1 meter Counter Domoticz           door: FlyingDomoticz
-- Opgeschoond/getest voor P1 Counter  door: Rnee
-------------------------------------------------------------------------------------------
-- Dit script leest de JSON waardes van DSMR Slimme Meter Logger en vult de P1 counter 
-- (op p1CounterIdx = 23xx) in Domoticz en kan zo gebruikt worden in het Energie dashboard.
-- -----------------------------------------------------------------------------------------
-- Script 
-- -----------------------------------------------------------------------------------------
local DSMR_IP = "192.168.2.102"      -- Ip-Adres ESP-DSMR Slimme Meter Logger of gebruik: 
                                     -- DSMR-API : dsmr-api.local 
Firmware      = "DSMR-API"           -- Vul in welke firmware je gebruikt. 
                                     --
domoticz_in_docker = "Ja"            -- Zet domoticz_in_docker op Ja als domoticz op Docker is 
                                     -- geinstalleerd. Keuze  "Ja" of "Nee" (hoofdlettergevoelig)
-- -----------------------------------------------------------------------------------------
commandArray = {}

if domoticz_in_docker=="Ja" then
    json = (loadfile "/opt/domoticz/config/scripts/lua/JSON.lua")()
else    
    json = (loadfile "scripts/lua/JSON.lua")()
end

    local jsondata    = assert(io.popen('curl http://'..DSMR_IP..'/api/v2/sm/actual'))
    local jsondevices = jsondata:read('*all')
    jsondata:close()
    
    local jsonCPM = json:decode(jsondevices)

    Energy_Delivered_Tariff1 = tonumber(jsonCPM.energy_delivered_tariff1.value)
    Energy_Delivered_Tariff2 = tonumber(jsonCPM.energy_delivered_tariff2.value)
    Power_Delivered =  jsonCPM.power_delivered.value
    Energy_Returned_Tariff1 = tonumber(jsonCPM.energy_returned_tariff1.value)
    Energy_Returned_Tariff2 = tonumber(jsonCPM.energy_returned_tariff2.value)
    Power_Returned = tonumber(jsonCPM.power_returned.value)
    
    p1CounterIdx = 2303
    counterData =  string.format("%d|0|%d;%d;%d;%d;%d;%d", p1CounterIdx, Energy_Delivered_Tariff1 * 1000, Energy_Delivered_Tariff2 * 1000, Energy_Returned_Tariff1 * 1000, Energy_Returned_Tariff2 * 1000, Power_Delivered * 1000, Power_Returned * 1000)
    table.insert (commandArray, {['UpdateDevice'] = counterData})
    
    Gas_Delivered = tonumber(jsonCPM.gas_delivered.value)*1000
    gasCounterIdx = 2304
    commandArray[#commandArray+1] = {['UpdateDevice'] = ''..gasCounterIdx..'|0|'..Gas_Delivered..''}
    
    
return commandArray
