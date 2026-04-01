# Earthquake Detection and Alert System ⚠️

##  Overview
The Earthquake Detection and Alert System is an IoT-based solution designed to detect seismic activity in real time and send instant alerts to users. This project integrates embedded systems with cloud computing to provide an efficient and scalable early warning system.

##Key Features
- Real-time earthquake detection using accelerometer sensors
- Cloud-based data processing using AWS services
- Instant alert notifications via SMS
- Low-cost and scalable architecture
- Remote monitoring capability

##  Technologies Used
- Arduino Uno (Microcontroller)
- ADXL345 Accelerometer Sensor
- ESP8266 WiFi Module
- AWS Services:
  - API Gateway
  - AWS Lambda
  - Amazon SNS (Simple Notification Service)

## System Architecture
1. The accelerometer detects ground vibrations across X, Y, and Z axes.
2. Arduino processes the sensor data.
3. ESP8266 sends the data to AWS via HTTP requests.
4. API Gateway receives and forwards the data to AWS Lambda.
5. Lambda analyzes the data based on predefined thresholds.
6. If abnormal activity is detected, SNS sends alert notifications to users.

## Workflow
- Sensor → Arduino → ESP8266 → API Gateway → Lambda → SNS → User Alert

## Alert System
When seismic activity exceeds safe limits:
- SMS alerts are sent instantly
- Users receive safety instructions and precautions

##  Applications
- Earthquake early warning systems
- Disaster management
- Smart city safety systems
- Industrial vibration monitoring

## Advantages
- Real-time monitoring
- Serverless and scalable backend
- Cost-effective implementation
- Fast alert delivery system

## Future Enhancements
- Mobile application integration
- Machine learning for improved accuracy
- Multi-location sensor deployment
- Dashboard for real-time visualization

##  Conclusion
This project demonstrates how IoT and cloud technologies can be combined to build an effective earthquake detection and alert system, improving public safety and disaster preparedness.








<img width="795" height="1368" alt="image" src="https://github.com/user-attachments/assets/4b58d89b-1ca9-4cbc-84c9-10beaa75acf6" />
 
Code for connecting wifi
 
const char* ssid = "hotspot_010";
const char* password = "Root@02";
 
WiFi.begin(ssid, password);
 
while (WiFi.status() != WL_CONNECTED) {
  delay(1000);
  Serial.print(".");
}
 



GATEWAY
 
 
const char* serverName = "https://adx14.execute-api.ap-south-1.amazonaws.com/prod/data";
 
void setup() {
  Serial.begin(115200);
  Wire.begin();
 
   mpu.initialize();
  if (mpu.testConnection()) {
    Serial.println("MPU6050 Connected");
  } else {
    Serial.println("MPU6050 Connection Failed");
  }
 
  // Connect WiFi
  WiFi.begin(ssid, password);
  Serial.print("Connecting");
 
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.print(".");
  }
 
  Serial.println("\nWiFi Connected");
}
 
void loop() {
  int16_t ax, ay, az;
 
  mpu.getAcceleration(&ax, &ay, &az);
 
 
  float accel = sqrt(ax * ax + ay * ay + az * az) / 16384.0;
 
  Serial.print("Acceleration: ");
  Serial.println(accel);
 
 
  if (accel >= 3.0 && accel <= 9.5) {
 
   if (WiFi.status() == WL_CONNECTED) {
      HTTPClient http;
 
   http.begin(serverName);
      http.addHeader("Content-Type", "application/json");
 
    
   String jsonData = "{";
      jsonData += "\"acceleration\": " + String(accel, 2);
      jsonData += "}";
 
 
   int httpResponseCode = http.POST(jsonData);
 
   Serial.print("Response Code: ");
   Serial.println(httpResponseCode);
     http.end();
    }
  } else {
    Serial.println("Below threshold (Not sent)");
  }
 
  delay(2000);
}




import json
import boto3
 
sns = boto3.client('sns')
 
SNS_TOPIC_ARN = "arn:aws:sns:ap-south-1:123456789012:EarthquakeAlerts"
 
def handler(event, context):
    try:
        # Get JSON from API Gatew
        body = json.loads(event['body'])
        acceleration = body.get('acceleration', 0)
     print(f"Received acceleration: {acceleration}")
 
   if acceleration < 3:
        message = "No significant vibration detected."
        
   elif 3 <= acceleration < 5:
            message = (
                "⚠️ Mild Tremor Detected.\n"
                "Stay calm. Move away from windows and heavy objects."
            )
        
  elif 5 <= acceleration < 7:
            message = (
                "⚠️ Moderate Earthquake Detected.\n"
                "Take cover under a sturdy table. Stay indoors."
            )
        
   else:
            message = (
                "🚨 Strong Earthquake Detected!\n"
                "Drop, Cover, and Hold On!\n"
                "Evacuate to an open area after shaking stops."
            )
 
   print("Message to send:", message)
 
   # Send to SNS
  response = sns.publish(
            TopicArn=SNS_TOPIC_ARN,
            Message=message,
            Subject="Earthquake Alert 🚨"
        )
     print("SNS Response:", response)
 
  return {
            'statusCode': 200,
            'body': json.dumps({
                'status': 'success',
                'message_sent': message
            })
        }
 
 except Exception as e:
        print("Error:", str(e))
        return {
            'statusCode': 500,
     'body': json.dumps({'error': str(e)})
   }

