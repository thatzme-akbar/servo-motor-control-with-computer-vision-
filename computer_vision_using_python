import cv2
import mediapipe as mp
import serial
import numpy as np

# Function to calculate the angle between three points
def calculate_angle(a, b, c):
    a = np.array([a.x, a.y])
    b = np.array([b.x, b.y])
    c = np.array([c.x, c.y])

    radians = np.arctan2(c[1] - b[1], c[0] - b[0]) - np.arctan2(a[1] - b[1], a[0] - b[0])
    angle = np.abs(radians * 180.0 / np.pi)

    if angle > 180.0:
        angle = 360 - angle

    return angle

# Initialize MediaPipe hands and serial communication
mp_hands = mp.solutions.hands
hands = mp_hands.Hands(static_image_mode=False, max_num_hands=1, min_detection_confidence=0.7)
mp_drawing = mp.solutions.drawing_utils

# Start video capture
cap = cv2.VideoCapture(0)
ser = serial.Serial('COM6', 9600)  # Adjust COM port as needed

while True:
    ret, frame = cap.read()
    if not ret:
        break

    # Process frame and flip for mirror view
    frame = cv2.flip(frame, 1)
    rgb_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
    results = hands.process(rgb_frame)

    # Check if any hands are detected
    if results.multi_hand_landmarks:
        for hand_landmarks in results.multi_hand_landmarks:
            # Draw hand landmarks
            mp_drawing.draw_landmarks(frame, hand_landmarks, mp_hands.HAND_CONNECTIONS)

            # Define landmarks for index finger, thumb, and wrist
            index_finger_tip = hand_landmarks.landmark[mp_hands.HandLandmark.INDEX_FINGER_TIP]
            thumb_tip = hand_landmarks.landmark[mp_hands.HandLandmark.THUMB_TIP]
            wrist = hand_landmarks.landmark[mp_hands.HandLandmark.WRIST]

            # Calculate angle between index finger and thumb
            angle = calculate_angle(index_finger_tip, thumb_tip, wrist)

            # Set a threshold for turning on/off
            threshold_angle = 90
            if angle > threshold_angle:
                print("Fan ON")
                ser.write(b'1')  # Send '1' to Arduino
            else:
                print("Fan OFF")
                ser.write(b'0')  # Send '0' to Arduino

            # Draw visualization line (optional)
            cv2.line(frame, 
                     (int(index_finger_tip.x * frame.shape[1]), int(index_finger_tip.y * frame.shape[0])), 
                     (int(thumb_tip.x * frame.shape[1]), int(thumb_tip.y * frame.shape[0])), 
                     (255, 0, 0), 2)

    # Display video feed with landmarks
    cv2.imshow('MediaPipe Hands', frame)
    if cv2.waitKey(5) & 0xFF == 27:
        break

cap.release()
cv2.destroyAllWindows()
ser.close()
