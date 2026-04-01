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
<img width="795" height="1368" alt="image" src="https://github.com/user-attachments/assets/bb5d4243-0449-45b0-b77d-36d59a403879" />
