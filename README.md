My ESP32 Wi-Fi Garbage Cleaner Boat

Greetings all! This is my Hack Club project. It is a remote-control boat that glides above ponds to skim off floating plastics and garbage. It sets up its own wi-fi hotspot so you can access its web dashboard right at your local pond and control it using a webpage dashboard I coded.

It also has an ultra-sonic distance sensor over the bag of trash that automatically figures out what percentage the bag is full and pushes a live percentage readout up onto your phone's display so you know when your boat is full of garbage.

Super Cool Features

• Built In Hotspot: The boat creates its own wi-fi network called RC_Trash_Boat. You do not need a home-router or internet to operate it outside.

• Web Dashboard UI: There is no apps to install. You just type the IP address into your phone's web browser and immediately start using the button steering wheel panel.

• Trash Scanner Gauge: The sensor checks how high up the garbage pile is every second so you do not accidentally overflow the storage tank.

• Quick Upload Mode: I optimized the code using a special PROGMEM layout memory trick. This allows the program to compile in a few seconds and to not have the computer lag or freeze-up.

The Parts I Used

To build this skimmer you will need an ESP32 Development Board (DevKit V1), L298N Motor Driver Board, HC-SR04 Ultrasonic Sensor, and a standard Servo Motor such as an SG90 or MG996R. For propulsion it uses two toy DC motors. Everything is powered by a battery pack (7.4V or 12V Li-Po or 18650 cells), routed through a Buck Converter Module, which is a step-down voltage regulator.

How I Wired It All Up

Before connecting any components there is one step needed on hardware boards. You must completely remove the black plastic jumper caps from the ENA and ENB pins on the motor driver, but make sure to keep the jumper cap firmly on for the 5V_EN pins.

Power and Ground Sharing

First, connect the Battery Positive (Red wire) into both the L298N 12V Port and the Buck Converter IN+ terminal. Next, route the Battery Negative (Black wire) into the L298N GND Port and the Buck Converter IN- terminal.

Take your Buck Converter and turn the manual dial until the output voltage reads exactly 5V. Connect this Buck Converter OUT+ line into the ESP32 VIN Pin, the Ultrasonic Sensor VCC pin, and the Servo Motor Red Wire. Finally, connect the Buck Converter OUT- line directly into an ESP32 GND Pin. This sets up the mandatory common ground bridge so all the chips can talk to each other cleanly.

Logic Pins and Brain Wires

To handle the brain-signals connect the L298N ENA (Left Speed) pin into ESP32 GPIO 25, making sure to insert the jumper-wire into the inner metal pin. Route L298N IN1 and IN2 over into ESP32 GPIO 26 and GPIO 27. Then, link L298N IN3 and IN4 into ESP32 GPIO 32 and GPIO 33. Connect the L298N ENB (Right Speed) pin into ESP32 GPIO 18, utilizing the inner metal pin. For steering plug the Servo Signal Wire (Orange or Yellow) to ESP32 GPIO 13. Lastly, connect the Ultrasonic Sensor Trig and Echo pins to ESP32 GPIO 12 and GPIO 14.

Motor Outputs

Wiring the actual propulsion is simple. Take your Left Propeller Motor wires and screw them into the L298N side terminals marked OUT1 and OUT2. Then, take your Right Propeller Motor wires and screw them into the opposite side's terminals marked OUT3 and OUT4. As a tip if you press the forwards button on your phone on testing and a motor spins backwards, just loosen up the terminal-screws, flip those two motor wires around, and screw them back in tightly.

Software Setup

I used the standard Arduino IDE desktop app to flash the microcontroller script down to the board.

Libraries Needed

Open up the library manager sidebar by going to Tools, selecting Manage Libraries, searching for this exact name, and clicking install:

• ESP32Servo by Kevin Harrington

The normal Arduino servo library conflicts with the internal clocks of the ESP32 chip, so using this custom one is necessary. The other required communication utilities, WiFi.h and WebServer.h, are already embedded in the core ESP32 board installation files, so you do not need to download them manually.

3D Printing and Slicing Layout

I used an Anycubic Kobra 2 Neo printer to build the hull. Since the print bed size is 220x220mm, I went with a modular-hull design that is split into interlocking segments so that it fits on the build-plate.

When you slice the files in Cura or PrusaSlicer, make sure to turn your wall perimeters up to 4 or 5 walls so that the plastic skins melt completely together. If you don't, micro-gaps between print-layers will leak water into your electronics. Also use a 15% Gyroid Infill pattern. If your boat-hull ever crashes into a rock or a stick and cracks-open on the water, the gyroid geometric pockets safely trap the leak inside a single square section so that the whole ship does not flood and sink.

Driving the Boat

Operating the cleaner boat has a few quick steps:

1. Plug in your main battery pack, and you will see the small red status LEDs on your circuit boards light up.

2. Open up your smartphone Wi-Fi network settings page.

3. Look for the network name RC_Trash_Boat and connect to it using the password password123.

4. Open up Chrome, Safari, or whatever internet-browser app you prefer to use.

5. Type 192.168.4.1 into the web address navigation bar at the very top of your phone screen and hit enter.

6. Your custom controller dashboard will come-up instantly on your phone display. Press down on the arrow buttons to move, slide the rudder tracker to adjust your steering-angle, and enjoy cleaning up the water.

Open Source License

I released this project under the open-source MIT License. You are free to copy my code, alter the wiring layout, print a different hull design, and head out to clear the plastic waste out of your local rivers and lakes!
