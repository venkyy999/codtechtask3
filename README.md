# codtechtask3
🟣 1. Introduction:
This project aims to create a smart surveillance system that can automatically capture a photo when motion is detected and send it to a mobile phone. It utilizes a Raspberry Pi, a motion sensor (PIR), and a camera. The captured photo is then routed to a phone through Telegram Bot API.

Such a system is useful for securing homes, offices, and small businesses, allowing real-time alerts and snapshots directly on a phone, regardless of where you are.

🟣 2. Scope:
Security: The system can help to instantly notify a person about a movement in their home or office.

Surveillance: Allows you to view live snapshots of a space without needing a permanent video stream.

Automation: The process — from motion detection to photo delivery — is fully automated.

🟣 3. Components:
✅ Hardware:

Raspberry Pi 4 or 3

Raspberry Pi Camera Module

PIR Motion Sensor

Breadboard and Jumper Wires

Power Supply

SD Card with OS

✅ Software:

Raspberry Pi OS

Python 3

RPi.GPIO (General-purpose I/O control)

Picamera (For accessing camera)

Telepot (Telegram Bot API wrapper)

🟣 4. Block Diagram:

 [ Smartphone (Telegram App) ]
🟣 5. Working Principle:
➥ The PIR sensor constantly monitors its surroundings.
➥ Whenever it detects motion, it signals the Raspberry Pi's GPIO.
➥ The Raspberry Pi then activate its camera and capture a photo immediately.
➥ The photo is temporarily stored in the Raspberry Pi's memory.
➥ Subsequently, the photo is sent to a predefined Telegram Bot chat alongside a notification message.
➥ The phone (with the Telegram App) instantly receives the photo and the alarm message.

🟣 6. Installation and Implementation:
6.1 Hardware Connection:
Component	GPIO	Connection
PIR	GPIO 17	output
Camera	camera port	ribbon
5V, GND	—	connect to 5V and Ground

6.2 Software Installation:
➥ Update and install required packages:

shell
Copy
Edit
sudo apt update
sudo apt install python3-pip
pip3 install RPi.GPIO picamera telepot
➥ Enable camera in raspi-config.

shell
Copy
Edit
sudo raspi-config
➥ Enable “Camera” under interfacing options.

🟣 7. Source Code:
python
Copy
Edit
import RPi.GPIO as GPIO
import time
import datetime
from picamera import PiCamera
import telepot

# Initialize components
GPIO.setmode(GPIO.BCM)
PIR = 17
GPIO.setup(PIR, GPIO.IN)

camera = PiCamera()
bot = telepot.Bot('YOUR_API_KEY')
chat_id = YOUR_CHAT_ID

print("Security camera initialized.")
try:
    while True:
        if GPIO.input(PIR):
            timestamp = datetime.datetime.now().strftime("%Y-%m-%d_%H-%M-%S")
            photo_file = f"/home/pi/{timestamp}.jpg"

            camera.capture(photo_file)

            bot.sendMessage(chat_id, "Motion Detected.")
            with open(photo_file, 'rb') as photo:
                bot.sendPhoto(chat_id, photo)

            time.sleep(10)  # avoid repeated messages immediately
        time.sleep(1)

except KeyboardInterrupt:
    GPIO.cleanup()
    camera.close()
🟣 8. Advantages:
✅ Immediate phone alerts upon movement.
✅ Allows photo verification (you know exactly what triggered the alarm).
✅ Low-cost, lightweight, and power efficient.
✅ Reliable for home, office, and small businesses.

🟣 9. Applications:
➥ Home Security
➥ Office Surveillance
➥ Vacation Home Monitor
➥ Small Stores or Shops
➥ Pet or Baby Monitor

🟣 10. Conclusion:
This project successfully demonstrates how to combine a motion sensor, camera, and messaging service to create a low-cost, real-time surveillance and alarm system.
Using a Raspberry Pi makes it flexible and adaptable for future expansion — adding additional sensors, alarm signals, or even integrating face recognition — depending on the application's needs.

