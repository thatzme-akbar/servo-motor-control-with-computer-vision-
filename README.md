# servo-motor-control-with-computer-vision-
"A project to control a servo motor using hand gesture recognition with MediaPipe and OpenCV."
# Servo Motor Control Using Computer Vision

This project demonstrates how to control a servo motor using computer vision techniques with MediaPipe and OpenCV. By detecting hand gestures through a webcam, the code calculates the angle between the index finger and thumb. If the angle exceeds a set threshold, a command is sent to an Arduino to rotate a servo motor, effectively turning a "fan" on or off.

## Components
- **Python** for image processing and hand gesture recognition
- **MediaPipe** and **OpenCV** for real-time hand tracking
- **Arduino** with a servo motor for physical control

## Requirements
- Python 3.x
- [MediaPipe](https://google.github.io/mediapipe/) and [OpenCV](https://opencv.org/) libraries
- NumPy for array manipulation
- Arduino IDE for uploading code to the Arduino board

## Project Structure
servo-motor-control-with-computer-vision/
├── README.md              # Project documentation
├── servo_control.py       # Python code for computer vision and serial communication with Arduino
└── servo_control.ino      # Arduino code for controlling the servo motor based on serial commands
