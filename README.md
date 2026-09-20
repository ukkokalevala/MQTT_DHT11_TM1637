lightweight, publish-subscribe, machine-to-machine (M2M) network protocol.
Sketch 1: Publisher (DHT11 → MQTT)
 What it does:
• Reads temperature from a DHT11 sensor
• Connects to Wi-Fi and a public MQTT broker
• Publishes the temperature to a topic: "wemos/temp" every 5 seconds
 Key parts:
float t = dht.readTemperature();       // Read temperature
client.publish("wemos/temp", payload); // Send to broker
• Wi-Fi connects using your SSID/password
• MQTT broker is broker.hivemq.com (public server)
________________________________________
 Sketch 2: Subscriber (TM1637 Display)
 What it does:
• Connects to the same Wi-Fi and MQTT broker
• Subscribes to "wemos/temp"
• When a new message arrives, it displays the integer temperature (like 23°C) on a TM1637 4-digit display
 Key parts:
client.subscribe("wemos/temp");       // Listen to topic
void callback(...) {                  // Called when a message arrives
  float temp = atof(msg);            // Convert to float
  display.showNumberDec(tempInt...); // Show on TM1637
  display.setSegments(celsius...);   // Add °C symbol
}
________________________________________
 Summary: How They Work Together
Device Action Topic
Publisher Reads from DHT11, sends temp wemos/temp
Subscriber Listens for that temp, displays it wemos/temp
 Both use the same topic, and the MQTT broker routes the messages between them.

1. What MQTT is (in simple terms)
MQTT is a publish / subscribe messaging system.
• Devices do not talk directly to each other
• They talk through a broker
• The broker acts like a post office
________________________________________
2. What the broker does
broker.hivemq.com is a public MQTT broker hosted by HiveMQ.
Its job is to:
• Receive messages from devices (publishers)
• Forward messages to interested devices (subscribers)
• Match messages using topics
It does not store your data permanently or know what it means.
