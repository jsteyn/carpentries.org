---
layout: page
authors: ["Jannetta S. Steyn", "Samantha Finnigan"]
teaser: "How not to have a nervous breakdown when running a workshop"
title: "Teaching the Introduction to the Internet of Things lesson"
date: 2023-03-02
time: "09:00:00"
tags: ["Workshops", "Internet of things", "Incubator", "Instructors"]
---

In the post-workshop survey, a learner responded to the question "Please provide an example of how an instructor or helper affected your learning experience" with this comment:

> I had a question that I was shy about, when they were faced with a similar issue they smiled and said, "we don't know the answer as well, let's search for it". It was really enjoyable to be around them.

To [me](https://jannetta.com), as a Carpentries instructor, this is not just the epitome of a complement, but also captures the essence of our workshops.

The [An Introduction to the Internet of Things (IoT)](https://carpentries-incubator.github.io/iot-novice/) lesson is still in [The Carpentries Incubator](https://carpentries-incubator.org/) and this was the second time I had the opportunity to present it. The content of this lesson, in particular, presents some big challenges. In my introduction to the workshop, I told the learners to expect things **not** to go smoothly. All the things that were going to go wrong, however, were to serve as part of the learning experience. I am sure many will nod their heads in agreement if I say that the worst thing you can do during a presentation is to try and deliver a live demo - of any sort really - because if you create an opportunity for chaos to creep in, it will. In a workshop such as this one, the laws of chaos govern.

In case you think I'm exaggerating or if you think I am silly for having been particularly nervous about this workshop, I'll share with you my experience of a few weeks ago when I decided to run an IoT taster session during our Code Community meetup. The meetup was also supposed to serve as a test-run for the one-day workshop.

Code Community was scheduled for 12:00 to 14:00 so I had the morning to make sure everything was still working. I created a [RaspAP](https://raspap.com/) [access point](https://en.wikipedia.org/wiki/Wireless_access_point) and an [MQTT](https://mqtt.org/) server from a [Raspberry Pi 4](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/) (RPi) with 2GB RAM. All I had to do was switch the RPi on. I tested it and it was working well. I could connect my laptop to RaspAP and I could also upload code from the [Arduino IDE](https://www.arduino.cc/en/software) on my laptop to an [ESP](https://www.espressif.com/sites/default/files/documentation/esp32-wroom-32_datasheet_en.pdf) and the ESP32 would connect to the RPi. The ESP32 would publish some information to the MQTT server and, from my laptop using MQTTx, I could subscribe to the published topic.

![ESP32](https://upload.wikimedia.org/wikipedia/commons/thumb/2/20/ESP32_Espressif_ESP-WROOM-32_Dev_Board.jpg/640px-ESP32_Espressif_ESP-WROOM-32_Dev_Board.jpg)
*An ESP32 Wroom 32 microcontroller*

At 11:30, all was shut down and packed up in my very [fancy waterproof case](https://www.peli.com/eu/en/product/cases/carry-on-case/air/1535) with wheels and a retractable handle. A colleague gave me a hand with all that needed lugging to the other side of our campus where our venue for Code Community was. I even remembered my multi-plug extension towers - three of them giving 24 sockets. Each of the 12 people that signed up could plug their laptops in twice.

The session started. I emailed folks beforehand, asking them to bring their laptops and install the Arduino IDE and [MQTTx](https://mqttx.app/). Some did and some didn't. That's okay, I figured, they can just watch or work with someone else. I handed out the ESP32s and switched the RPi with the RaspAP and MQTT servers on. So far so good. From here on though, everything went wrong. My laptop which was in the very fancy case would not start up. The rattling of the case when we wheeled it across campus caused the external NVMe SSD, which it was meant to boot from, to dislodge inside it's caddie.

No worries, I thought, I brought my other laptop. But, alas, the workstation connected to the large screens in front of the room was missing an HDMI cable so I could not plug it in. Obviously, I could not use the workstation connected to the screens because it was connected to the network with an Ethernet cable, and it did not have WiFi. I could, I thought, just talk people through the process so I asked them to connect to the RaspAP and gave them the SSID and password. But, alas again, they could not connect, so the obvious problem had to be the password, but I checked, and I had it right.

I pre-programmed the ESPs to connect to the RaspAP and then to publish "hello" to the MQTT server. While I was struggling with laptops one of the guys, Ben, who had some experience with microcontrollers managed to connect the ESP to his computer and noticed that the ESP did actually connect to both the RaspAP and MQTT server. Suspecting that the password I gave them was still incorrect, he dumped the code from the ESP to the computer and was able to spot the SSID and password in the dumped code. The password I gave them was right, but despite this confirmation, none of the laptops would connect. Ben then copied and pasted the password from the dumped code to his laptop and to our amazement the laptop connected. He even emailed the copied password to someone else and at that point and they too could connect. If you pasted the copied password, the computer would connect but if you typed the password, it would not.

By now people were helping one another and a few managed to upload code to the ESP and connect some sensors and LEDs to the ESPs. I managed to talk a bit about the pre-loaded code and how the OTA code was meant to allow people to upload new code to the ESPs over the air without physical access to the controllers. I also explained a bit about what MQTT was and how it could be used. At least the meetup was not a complete waste of time. People's interest was captured, and a few showed an interest in attending the IoT workshop which would be a few weeks later. The password mystery is still a mystery. After some contemplation and discussion, we decided that the probable cause was a hyphen in the password that was encoded differently. Despite confirming this not to be the case in the dumped code it still seemed the most feasible explanation.

Back to the main workshop. Everything that went wrong before worked perfectly well this time round. There was an HDMI cable on the workstation connected to the big screens in front of the room. Only one person did not bring a laptop and could share with another student and only a couple of people did not install the required software, so the helpers sorted them out quickly. I made sure there was no hyphen in the password anymore and all the computers were able to connect to the access point. Obviously, there was more than enough other things that could go wrong - and they did.

Here follows a list of those things and what we did to fix them:

1. Since I created the lesson, less than a year ago, the Arduino IDE interface changed quite a bit so the screenshots of the software in the lesson did not reflect what the students were seeing. Thanks to the HDMI cable connected to the big screens, we could project our laptop installations so that students could follow.

2. People using Linux may need to install the `pyserial` Python module to flash code to the ESP32. This is not well documented, as indicated by the fact that there are quite a few posts addressing this when you start googling the problem. The esp32 Board Manager includes [esptool.py](https://github.com/espressif/esptool) to handle flashing from the Arduino IDE. Esptool is run using the system Python installation. Some users who already work with Python (e.g. Conda users) may have a virtual environment set up by default in their `$PATH`, which you only discover when the student installs the `pyserial` module, which goes into their virtual environment, not system python, and the error persists. You can diagnose this using `which python` from a shell. Assuming that the Arduino IDE would be using the system python installation, we then suggested installing the module with `sudo python3 -m pip install pyserial` which solved the problem. Modifying system python is probably not the ideal solution, but it was the quickest.

3. Unlike Macs and Linux PCs, Windows PCs need to install the `CP210x USB to UART Bridge VCP Drivers` if one uses the ESP32 microcontroller boards. The instructions for doing it are in the notes, but if you accidentally skip that paragraph or get asked by a student why, unlike their neighbour, they cannot see the serial port before you get to that part in the lesson - do not forget that that might be the problem.

4. At our university literally all the sockets in PC clusters are taken up by the PCs in the cluster. There might be an odd socket located at least three to five meters away from the nearest desk. In meeting rooms, there are usually no sockets on the floor and probably one or two sockets in the wall at the opposite ends of the room also, at least, two meters away from the nearest seat. So, do not forget to take several strip plugs. I bought three multi-plug towers which can take eight plugs each. But you might find that, depending on the room layout, eight people cannot reach the tower because of the tower's cord length and the lengths of the laptop cords. There are, thus, three more towers on order.

A little note: It is also nice if the towers, or strip plugs, have USB ports. You can then have students plug the microcontrollers into the USB port after it was programmed from the laptops, to prove that, after programming, the microcontroller can run on its own without being connected to a computer.

6. Then there was also the issue that, in the new Arduino IDE which implements intellisense, the intellisense will not work under certain circumstances. It appears that when the IDE references a directory path with non-English (such as Arabic) characters, then it cannot find that path. Although the intellisense will not work it does not seem to affect the compiler. Code will still compile.

7. If the default path used by the Arduino IDE for libraries contains non-English characters, the IDE won't find the libraries. To fix the problem the Sketchbook location can be changed in the preferences (i.e. Files, Preferences, Settings).

You have to realise with a workshop such as this one that things are going to go wrong, even more so than with the usual workshops, and you can't prepare for it all. This, for someone like me, is a hard pill to swallow. I'm the kind of person that walks around with a backpack that, besides the usual laptop, tablet and phone also contains a Swiss Army pocketknife (that big one with a saw and scissors and all kinds off stuff that you might need), and a pocket-size titanium cutlery set which includes a straw and chopsticks. There is also a fingertip microscope, several versions of USB cables and at least one USB power bank. I wear a paracord bracelet because you never know when you are going to need 9 feet of rope to save the day. For some reason though, I am always out of paracetamol...

However, by unapologetically warning folks that things are going to go wrong I managed to convince myself that I have justified our unpreparedness for any unforeseen challenges that the universe may throw our way and I really enjoyed the workshop. I do, however, have to acknowledge my co-instructor, [Samantha Finnigan](https://finnigan.dev/), who knows a lot more about IoT than me. She is also younger, with a better memory and googles a bit faster than me. Without much planning or pre-workshop discussions, we worked together really well. I would present the lesson and Samantha would drive the Arduino IDE displayed on the big screen for the students to follow and vice versa. I also had two marvellous helpers, Yash Borikar and Stuart Lewis. Between the four of us I believe that, based on the workshop feedback, we presented a successful and enjoyable workshop.

Sometimes, we do receive seemingly contradictory feedback, like the following two comments about ways the workshop could be improved:

- More detailed explanation of the code used in this workshop would be nice.
- Considering that this is an IoT workshop and not a programming workshop, less time could have been spent on the programming and instead focus more on practical examples of IoT.

Could this mean that we got it exactly right? Perhaps we can remember to ask the patience of those more proficient learners in the workshop introduction: there are always a mix of people with different abilities at these workshops, and some will appreciate deeper explanations of things that others may find trivial.

If you are interested in presenting or contributing to the lesson, please find it in the Carpentries Incubator at [https://github.com/carpentries-incubator/iot-novice](https://github.com/carpentries-incubator/iot-novice).


