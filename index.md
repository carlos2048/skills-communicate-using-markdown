# Este é o header 1
## E este o 2
###### Este é o 6

Isto foi para experimentar vários headers

![Image of Yaktocat](https://octodex.github.com/images/yaktocat.png)

```
$ git init
Initialized empty Git repository in /Users/skills/Projects/recipe-repository/.git/
```

``` javascript
var myVar = "Hello, world!";
```

# Este é o yaml do ESP32

```
############################################
# BASE CONFIGURATION
############################################

esphome:
  name: sala6
  friendly_name: sala6

esp32:
  board: esp32dev
  framework:
    type: esp-idf
    

# Enable logging
logger:
  level: DEBUG
#  level: VERBOSE

# Enable Home Assistant API
api:
  encryption:
    key: "7E7if0iUNNy1m+l8QBTy6evi3ajM7XufPaqPltQkJGg="

ota:
  - platform: esphome
    password: "e90174e0a1c8e65ea755d33a83d48c05"

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "Sala6 Fallback Hotspot"
    password: "tpYXMmPEr72t"

captive_portal:

############################################
# ACTIVE FEATURES
############################################

# Enable HTTP request
http_request:
  verify_ssl: false

esp32_ble_tracker:
#Thus, if you only use such sensors, you can safely set scan_parameters.active: false in esp32_ble_tracker configuration, 
#to save from spamming your RF environment with useless scan requests.
  scan_parameters:
    active: false
# Example configuration entry for finding MAC addresses, Service UUIDs, iBeacon UUIDs, and identifiers
#  on_ble_advertise:
#      then:
            
xiaomi_ble:

#web_server:
#  port: 80
#  version: 2

############################################
# SENSORS AND STUFF
############################################


#interval:
#  - interval: 30s
#    then:
#      - http_request.get:
#          url: "https://script.google.com/macros/s/AKfycbzskFj4ZZpyj-FIw5DbgBPXM2j6P7G7e2_-_IJ7sP3q7hjQ2goVQh-VFPeay4VVS3T4/exec?sensor_name=aaa&sensor_value=123456"
#          on_response:
#            then:
#              - logger.log: "Website is reachable."
#          on_error:
#            then:
#             - logger.log: "Website is NOT reachable."

output:
  - platform: gpio
    pin: GPIO2
    id: led_builtin

light:
  - platform: binary
    name: "ESP32 LED"
    output: led_builtin

sensor:
#  - platform: wifi_signal
#    name: "ESP32 Wi-Fi Signal Strength"
#    update_interval: 10s
#    on_value:
#      then:
#        - http_request.get:
#            url: !lambda |-
#              return ((std::string) "https://script.google.com/macros/s/AKfycbzskFj4ZZpyj-FIw5DbgBPXM2j6P7G7e2_-_IJ7sP3q7hjQ2goVQh-VFPeay4VVS3T4/exec?sensor_name=2%20Temperature&sensor_value=" + to_string(x));
#            url: "https://example.com"
    
# 2
  - platform: xiaomi_lywsd03mmc
    mac_address: A4:C1:38:37:05:C3
    bindkey: "22cdb2b150f181e8f883f63dd3ea04dc"
    temperature:
      name: "2 Temperature"
      id: t2
      filters:
        - round: 1 # will round to 1 decimal place
        - delta: 0.1 # only passes incoming values through if incoming value is sufficiently different from the previously passed one
      on_value:
        then:
          - http_request.get:
              url: !lambda |-
                return ((std::string) "https://script.google.com/macros/s/AKfycbwQj7xCo_M5VjtHOlkfZrC9wiBYuAWFyHeRin_nVHrUwIE1vK35Wc0SbEIJRNbBGq3e/exec?sensor_name=T2&sensor_value=" + to_string(std::round(x * 10) / 10));
          - logger.log:
              format: "T2 = %f"
              args: [ 'id(t2).state' ]
    humidity:
      name: "2 Humidity"
      filters:
        - round: 0 # will round to 1 decimal place
        - delta: 1 # only passes incoming values through if incoming value is sufficiently different from the previously passed one
      on_value:
        then:
          - http_request.get:
              url: !lambda |-
                return ((std::string) "https://script.google.com/macros/s/AKfycbwQj7xCo_M5VjtHOlkfZrC9wiBYuAWFyHeRin_nVHrUwIE1vK35Wc0SbEIJRNbBGq3e/exec?sensor_name=H2&sensor_value=" + to_string(x));
    battery_level:
      name: "2 Battery Level"

# 4
  - platform: xiaomi_lywsd03mmc
    mac_address: A4:C1:38:65:C7:9C
    bindkey: "df4642481b8e9bf01ece094e660f225a"
    temperature:
      name: "4 Temperature"
      id: t4
      filters:
        - round: 1 # will round to 1 decimal place
        - delta: 0.1 # only passes incoming values through if incoming value is sufficiently different from the previously passed one
      on_value:
        then:
          - http_request.get:
              url: !lambda |-
                return ((std::string) "https://script.google.com/macros/s/AKfycbwQj7xCo_M5VjtHOlkfZrC9wiBYuAWFyHeRin_nVHrUwIE1vK35Wc0SbEIJRNbBGq3e/exec?sensor_name=T4&sensor_value=" + to_string(std::round(x * 10) / 10));
          - logger.log:
              format: "T4 = %f"
              args: [ 'id(t4).state' ]
    humidity:
      name: "4 Humidity"
      filters:
        - round: 0 # will round to 1 decimal place
        - delta: 1 # only passes incoming values through if incoming value is sufficiently different from the previously passed one
      on_value:
        then:
          - http_request.get:
              url: !lambda |-
                return ((std::string) "https://script.google.com/macros/s/AKfycbwQj7xCo_M5VjtHOlkfZrC9wiBYuAWFyHeRin_nVHrUwIE1vK35Wc0SbEIJRNbBGq3e/exec?sensor_name=H4&sensor_value=" + to_string(x));
    battery_level:
      name: "4 Battery Level"

#binary_sensor:
#  - platform: status
#    name: "ESP32 Device Status"



```
