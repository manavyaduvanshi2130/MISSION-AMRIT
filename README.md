# MISSION-AMRIT
WATER QUALITY MONITORING SYSTEM
import RPi.GPIO as GPIO
import wifi
import ADS1x15
import time
import json

# WiFi credentials
SSID = "your_wifi_ssid"
PASSWORD = "your_wifi_password"

# Sensor pins
pH_Pin = 0
Turbidity_Pin = 1
Temperature_Pin = 2

# Sensor objects
pH_Sensor = ADS1x15.ADS1115(address=0x48)
Turbidity_Sensor = ADS1x15.ADS1115(address=0x49)
Temperature_Sensor = ADS1x15.ADS1115(address=0x4A)

# WiFi connection
wifi.connect(SSID, PASSWORD)

while True:
    # Read sensor values
    pH_Value = pH_Sensor.read(pH_Pin)
    Turbidity_Value = Turbidity_Sensor.read(Turbidity_Pin)
    Temperature_Value = Temperature_Sensor.read(Temperature_Pin)

    # Print sensor values
    print("pH: ", pH_Value)
    print("Turbidity: ", Turbidity_Value)
    print("Temperature: ", Temperature_Value)

    # Send data via WiFi
    data = {
        "pH": pH_Value,
        "Turbidity": Turbidity_Value,
        "Temperature": Temperature_Value
    }
    json_data = json.dumps(data)
    wifi.send(json_data)

    # Wait for 10 seconds
    time.sleep(10)
