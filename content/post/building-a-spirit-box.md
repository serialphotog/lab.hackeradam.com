---
title: "Building a Homemade Spirit Box"
description: "Notes from designing and building a spirit box for my mother."
date: '2021-04-18'
tags: [Electronics, Arduino, 3d-Printing]
draft: false
---

I am certainly no believer in the paranormal. My mother, on the other hand, is a different story. As such, she recently requested that I design and build a spirit box for her. Being that I am not one to turn down an opportunity to tinker on a new project, I decided to oblige. 

# What the Hell is a Spirit Box?

The first quest I needed to answer in this project is what in the hell is a spirit box anyway? 

Well, as it turns out, a spirit box is simply an electronic device that quickly switches through FM radio stations. This results in a weird, white-noise like pattern that some believe the spirit world can utilize to communicate. I'm not sure about that last bit, but the first part certainly doesn't sound too terribly difficult!

# Parts

* [SparkFun FM Tuner Breakout](https://www.sparkfun.com/products/11083)
* [SparkFun Mono Audio Amplifier Breakout](https://www.sparkfun.com/products/11044)
* [Arduino Pro Mini 328 5V](https://www.sparkfun.com/products/11113)
* [0.5W Speaker](https://www.sparkfun.com/products/9151)

# Test Electronics Setup

![The initial prototype of the spirit box electronics](/blog/spiritBoxPrototype01.jpg)

To begin, I wired everything up to an Arduino Uno to do the basic prototyping. The setup is pretty straightforward, with the arduino controlling the FM tuner. The speed at which the Arduino scans through the channels is controlled via a potentiometer. Here is a brief video of it running to give you an idea of what a spirit box does:

{{< youtube 6YfjLhG39iY >}}

The firmware for this initial prototype is quite simple:

```c
#include <Si4703_Breakout.h>
#include <Wire.h>

#define DEBUG

#define CHANNEL_LOW 0
#define CHANNEL_HIGH 700

int reset = 2;
int SDIO = A4;
int SCLK = A5;

int speed = 1;

Si4703_Breakout radio(reset, SDIO, SCLK);
int channel;

void setup() {
  #ifdef DEBUG
  Serial.begin(9600);
  #endif

  radio.powerOn();
  radio.setVolume(14);

  // Speed input
  pinMode(A0, INPUT);
}

void loop() {
  speed = analogRead(A0) / 2;
  if (speed <= 0) speed = 1;

  Serial.println(speed);
  
  channel++;
  if (channel > CHANNEL_HIGH)
    channel = CHANNEL_LOW;
  radio.setChannel(channel);
  delay(380 - speed);
}
```

Note that this firmware is making use of the [Si4703 breakout library](https://github.com/sparkfun/SparkFun_Si4703_Arduino_Library/tree/V_1.2.0) from SparkFun to control the FM board. 

## Wiring

The pinout is as follows:

#### Audio Amplifier:

* Ground --> Input (-)
* Ground --> Si4703 Ground
* PWR --> Si4703 VCC
* Out (+/-) --> Speaker
* In (+) --> Si4703 LOUT

***NOTE:*** I also added a pair of 10k resistors to the amplifier to increase the audio gain.

#### Si4703

* LOUT --> Amp In(+)
* Ground --> Arduino GND
* Ground --> Amp GND
* VCC --> Amp PWR
* VCC --> Arduino 5V pin
* SDIO --> Arduino A4
* SCLK --> Arduino A5
* RST --> Arduino D2
* GPIO2 --> Arduino D3

#### Arduino Uno

* D2 --> Si4703 RST
* D3 --> Si4703 GPIO2
* A4 --> Si4703 SDIO
* A5 --> Si4703 SCLK
* 3.3V --> Potentiometer (+)
* GND --> Potentiometer (-)
* A0 --> Potentiometer Output

# TODO:

This is an ongoing project that still needs quite a bit of work and refinement. Currently on the todo list are:

* Design & 3D print a case to hold the electronics.
* More permanently route the electronics in said case.
* Add a volume control potentiometer to the audio amplifier.