# Raspberry Pi: Adafruit 1000c PowerBoost Low Battery Safe Shutdown Script

This is a simple python script that safety shutdowns a [Raspberry Pi](https://www.raspberrypi.org/) running [Linux](https://www.raspberrypi.com/software/) (Ubuntu) running off of a [Adafruit PowerBoost 1000c smart LiPo battery charger](https://www.adafruit.com/product/2465). The Adafruit Powerboost 1000c is a smart charger that can manage a Lithium Polymer battery and smart charging capabilities; on the board, there is a pinout called "LBO" (or low battery out); this pin output a current at the given current/voltage levels as the battery but it goes to zero when the battery is low. Using this principle, a wire can be soldered to this pinout and connected to a GPIO pin on the raspberry pi, and the script can print a terminal message warning of a shutdown in a short period of time and then issues a system command to safety shutdown the Raspberry Pi (this is useful to prevent OS/data corruption from a system shutting off due to a dead battery). The script (along with the old versions) is available in the leading directory in this repository.

## How to use:
Download the [source code](https://github.com/Austin-Daigle/Raspberry-Pi-PowerBoost-1000c-Safe-Shutdown-Script/blob/main/Adafruit%20shutdown%20script.py) from the repo, and modify the code as needed (change shutdown times, GPIO hardware settings, etc). Load the script into an installation of Linux in a Raspberry Pi, physically connect the systems (RPI and PowerBoost together), and set a chron job to have the program start on boot.

Also: Older experimental developement version of the script are avaible [here](https://github.com/Austin-Daigle/Raspberry-Pi-PowerBoost-1000c-Safe-Shutdown-Script/tree/main/Old%20Versions).

The white wire is the low battery out ("LBO") terminal on the PowerBoost 1000c.
![a42d9674d56924ce-photo](https://user-images.githubusercontent.com/100094056/193486125-56772d65-1140-4c9c-aa8a-389c57dbe32c.jpeg)


## Here is the Python script:

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
