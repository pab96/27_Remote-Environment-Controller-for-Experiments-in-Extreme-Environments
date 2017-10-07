# Remote Environment Controller for  Experiments in Extreme Environments
Observing biological systems, animals, traffic or the evolution of cities in their natural environment requires researchers to travel to remote places. Field trips to these distant locations are often limited in time, expensive, or even dangerous. Here, we build an autonomous environment observer that once installed monitors key parameters that are essential for our research.
Our aim is to establish a sensor platform that is autonomous (i.e. solar powered) and able to transmit data and receive instructions remotely. Rather than targeting a single application, we build a generic sensor platform that can be used for a wide range of applications and can be easily adapted. Initially we test our system in a student vegetable garden where the sensor platform will be tested and simple measurements such as soil moisture, UV intensity, temperature, humidity, camera etc. are taken. The main outcome of this project will be a field tested generic sensor platform that can be easily adapted for a wide range of tasks. 

The below figure illustrates the data and power flow of the system:
![alt text](https://github.com/pab96/27_Remote-Environment-Controller-for-Experiments-in-Extreme-Environments/blob/master/LogicStructureRemoteSens.png)
In short, energy from a solar panel is feed into a battery. The battery powers the main processor (i.e. a raspberry pi). However, to save power the pi is only swtched on periodically by an arduini which requires significantly less enery to run and it can simultaniously monitor the battery voltage to prevent deep battery discharge and only swith on when the battery is sufficiently charged. The pi then read data from the sensors after the signal is converted from analog to digital. Finally the data is transmitted via wifi or the GSM (at locations without wifi) and can then be monitored from anywhere in the world with a PC.

The documentation is structured into several independent sub components which include:
- Solar Power and Batteries
- Processor (Raspberry Pi)
- Use of an external timer circuit to save power
- Choice of sensors and how to connect them
- Data transmission
- Mounting and weather resistant casing
- Data display and storage
- Future work

|  Component | Component Details |
| --- | --- |
| Solar panel and 12V battery charger | (RS Stock No.706-7918) |
| Battery | 12V Lead Acid Battery, 1.2Ah (RS Stock No.614-2447) |
| Arduino |Arduino Pro Mini |
| Raspberry Pi |Raspberry Pi 2B |
| Camera |Raspberry Pi Camera V2 Camera Module (RS Stock No.913-2664) |
| Power MOSFET |N-channel MOSFET, 5.6 A, 100 V (RS Stock No.708-5134) |


## Processor
We want to be able to possibly track a large number of sensors, take pictures and access a wifi. Doing this with an arduino will require quite a few boards and thus we decided to use an rasperry pi as the central processing unit. This comes at the disadvantage that a pi requires quite a lot of power, however we will overcome this by only switchen the pi on for limited periods.
First of all set up your pi. If you have never done this follow this quide: https://www.raspberrypi.org/app/uploads/2012/04/quick-start-guide-v2_1.pdf


### Components:
- A raspberry pi e.g. raspberry pi 2B
- For programming the pi you will need a keyboard and mouse, a screen (possibly a HDMI to VGA adapted depending on your screen)
- A SD card with the noobs operating system
- A wifi dongle
- power adapter

## Solar Power and Batteries:




Using a timer:
Data recording is only required every hour or so. In the intervalls in between Pi can be switched off which saves a lot of power. Power consumption is cruicial in tis project if the Pi is powered by a battery and a solar panel. So the idea is that the Pi only swictehs on every hour read the sensors and then switch off gain. The sleep mode requires too much power, thus an external timer device is used. This timer is described here:
Some useful online resources on this:
- http://www.circuitbasics.com/555-timer-basics-astable-mode/ -> Simple way of building and testing the timer.
- http://www.ti.com/lit/ds/symlink/lm555.pdf -> 555 timer datasheet

The Pi 2B has 2 tiny pins labelled run. If these pins are briefly short circuted the Pi will restart. We will use these two pins to restart the Pi after it was shut down by simply connecting them to a transistor which is then switched by the 555 timer. The 555 timer needs to send a signal every hour for a very shor pulse. Thus the so called A-Stable-Mode is used. The Astable mode is such that it is on for most of the time and switches off for a shorter time. Therefore we have to invert this signal such that the two "Run" pins on the Pi are generally off and only swicth on every hour for a short pulse.
R_1, R_2 and C of the 555 timer determine how long the on and off time are given by:
t_on= 0.693*(R_1+R_2)*C
t_off=0.693*(R_2)*C
A one hour on time is quite long and we need big transistors and capacitors to do that. We chose:
R_1=5.1MOhm + 47kOhm +3.3KOhm= 5150.3kOhm
R_2=1kOhm
C=1000 uF
This results in:
t_on=3569.85s
t_off=0.69s 

![alt text](https://raw.githubusercontent.com/pab96/GitHubGardenObserver/CircuitDrawings/TimerCircuit.png)



Multiplexing and analogue to digital conversion (ADC):
More info on setting up MCP3008: https://learn.adafruit.com/raspberry-pi-analog-to-digital-converters/mcp3008
Connect MCP3008 as follows:
MCP3008 VDD to Raspberry Pi 3.3V
MCP3008 VREF to Raspberry Pi 3.3V
MCP3008 AGND to Raspberry Pi GND
MCP3008 DGND to Raspberry Pi GND
MCP3008 CLK to Raspberry Pi pin 18
MCP3008 DOUT to Raspberry Pi pin 23
MCP3008 DIN to Raspberry Pi pin 24
MCP3008 CS/SHDN to Raspberry Pi pin 25
CHannel 0-7 will then read voltages from 0...3.3V and give a signal 0...1023 accordingly 





££££££££££££££££££££££££££££££££££££££££££££££££££££££££££££££££££££££££££££££££££££££££££££
Here are some loose guidlines meant to give you an idea of what information we expect to find in each repository. Feel free to present your documentation in the most accessible/suitable order and style but you **must** include project data relating to the headings below, if relevant to your project. Also, include your final proposal in the top directory.

[**A very good example**](https://github.com/Biological-Microsystems-Laboratory/micropipette)

Consider using [GitHub for desktop](https://desktop.github.com/), the user interface and experience is so much better than the web version of Github, in my opinion.

Lastly, follow [these](https://pages.github.com/) instructions if you want to style your github repository into a webpage like [so](https://biomakers.github.io/Example-repo/).

## Synopsis

A summary of your project. This is the 150 word description from your proposal. Include your project banner and photos of team members.

## Software

Explain functionality of software components (if any) as concisely as possible, developers should be able to figure out how your project solves their problem by looking at the code example. Ideally, this should be pseudo code or an abstract graphical representation of your code e.g entity relationship diagram. Consider adding a screenshot of your User Interface.

## Hardware

Explain how the hardware components (if any) of your project function as concisely as possible, including a short description of fabrication and assembly. Component suppliers and part numbers should be provided separately in a bill of materials, in a 'Hardware Folder'.

## Installation, Maintenance and Testing Guide

Provide instructions on usage, describe a test scheme and show how to run the tests with code and hardware configuration examples with some representative results.

## License

A short snippet describing the license (MIT, Apache, etc.) you have chosen to use
