import RPi.GPIO
import time
import os
RPi.GPIO.setmode(RPi.GPIO.BOARD)

RPi.GPIO.setup(7, RPi.GPIO.IN, pull_up_down=RPi.GPIO.PUD_UP)

while True:
  if RPi.GPIO.input(7) == RPi.GPIO.LOW:
      print ("Pin Low")
      time.sleep(10)
      print ("system: battery low ~ shutdown in 60 seconds.")
      time.sleep(60)
      os.system("sudo shutdown -h now")

  if RPi.GPIO.input(7) == RPi.GPIO.HIGH:
      now = time.strftime("%c")
      print ("Pin High " + now )
      time.sleep(60)
