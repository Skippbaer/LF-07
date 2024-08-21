# LF-07

 

WORKSHOP 

Installation Broker: 
sudo apt install mosquitto
sudo apt install mosquitto-clients 

Creating a Config for  mosquitto: 
cd /etc/mosquitto/conf.d 
sudo touch mosquitto.conf 

Write Data: 
(nano mosquitto.conf) 
 
per_listener_settings true  
listener 1896  
password_file /etc/mosquitto/conf.d/010-access-list 
allow_anonymous false 


Configure login data for the clients: 
sudo touch /etc/mosquitto/conf.d/010-access-list
sudo mosquitto_passwd /etc/mosquitto/conf.d/010-access-list <name>

Restart Server: 
sudo systemctl restart mosquitto.service 

Installation & Data transfer as Client: 


Login as Subscriber: 
mosquitto_sub -h <ip> -p 1896 -t “topic name” -u <User> -P <Password> 

Login as Publisher: 
mosquitto_pub -h <ip> -p 1896 -t “topic name” -m <Message> -u <User> -P <Password> 


MQTT und Python: 

To install paho-mqtt 

1. create a Virtual Environment: python -m venv < env-name > 

2. go into the Virtual Environment: source < your env-name >/bin/activate 

3. now install paho-mqtt: pip3 install paho-mqtt 

4. to get out of the Virtual Environment: deactivate 

 
```
##Subscriber: 

#Define the MQTT broker details 

broker = "172.20.167.40" 
port = 1896 
topic = "T" 

#Define the username and password 

username = "<user>" 
password = "<password>" 

#Define the callback function for when a message is received 

def on_message(client, userdata, msg): 
    print(f"Received message: {msg.payload.decode()} on topic {msg.topic}") 

Create a subscriber client instance 

client = mqtt.Client() 

Set username and password 

client.username_pw_set(username, password) 

Assign the on_message callback function 

client.on_message = on_message 

Connect to the broker 

client.connect(broker, port, 60) 

  

Subscribe to the topic 

client.subscribe(topic) 

Start the loop to process received messages 

try: 
    client.loop_forever() 
except KeyboardInterrupt: 
    print("Subscriber stopped.") 
finally: 
    # Disconnect the client if loop_forever is interrupted 
    client.disconnect() 

    
##Publisher:  

import paho.mqtt.client as mqtt 
import time   

#Define the MQTT broker details 

broker = "172.20.167.40" 
port = 1896 
topic = "T" 

#Define the username and password 

username = "<user>" 
password = "<password>" 

Create a publisher client instance 

client = mqtt.Client() 


Set username and password 

client.username_pw_set(username, password) 

  

Connect to the broker 

client.connect(broker, port, 60) 

  

Publish messages in a loop with a delay 

try: 
    while True: 
        message = "Hello, MQTT with authentication!" 
        client.publish(topic, message) 
        print(f"Published message: {message}") 
        time.sleep(5)  # Wait for 5 seconds before sending the next message 
except KeyboardInterrupt: 
    print("Publisher stopped.") 

finally: 

    # Disconnect the client 

    client.disconnect() 
```
