---
title: Modding a Crestron CNAMPX-16X60 amplifier
date: "2020-08-15T22:40:32.169Z"
template: "post"
draft: false
slug: "crestron-cnampx-16X60"
category: "Hardware"
tags:
  - "Hardware"
  - "Project"
description: "The Crestron CNAMPX-16X60 amplifier is an absolute beast of an amplifier, clocking in at 90 lbs and packing 16 channels of amplification at 60 watts each. You can find them on eBay for under 300 bucks too, but you have to figure out how to emulate Cresnet in order to actually use it. Read on to see how I made mine Wi-Fi and 12V trigger controlled."
socialImage: "/media/crestron/IMG_9950.jpeg"
---

![My cat performs the ritual swearing in of new AV gear by sitting on it.](/media/crestron/IMG_9950.jpeg)
> My cat performs the ritual swearing in of new AV gear by sitting on it.

# Table of Contents

- [Background](#background)
	- [Living Room](#living-room)
	- [Home Theater](#home-theater)
	- [So why upgrade?](#so-why-upgrade)
- [Enter the Crestron](#enter-the-crestron)
	- [Why's it so cheap?](#whys-it-so-cheap)
- [Modding the Creston CNAMPX-16X60](#modding-the-creston-cnampx-16x60)
	- [Cresnet Explanation](#cresnet-explanation)
	- [Powering on the amp](#powering-on-the-amp)
	- [Cracking it open](#cracking-it-open)
- [Modifying some XLR -> RCA cables](#modifying-some-xlr---rca-cables)
- [Argh! Ground Loop Interference!](#argh-ground-loop-interference)
- [Improving remote control](#improving-remote-control)
- [Future Enhancements](#future-enhancements)
	- [Audio Sensing](#audio-sensing)
	- [Cresnet Serial Control](#cresnet-serial-control)

# Background

I'm a huge home theater nut. It drove my house buying decision, I spent years planning my build, and another 9+ months actually doing all the build. I DIY'd everything I could, and cobbled together a collection of gear that offered some excellent value, but soon wanted an upgrade. 

At the time I had a pretty extensive collection of AV gear in my rack, and it powered two different systems in my home. 

## Living Room

![](/media/crestron/livingroom.jpg)

The simpler of the two is my stereo setup in my living room consisting of a pair of [Monitor Audio Bronze 5's](https://www.monitoraudio.com/en/support/past-products/bronze-5g/bronze-5/). They fit well into the room and I have binding posts nearby with speaker cables snaking down to the basment AV rack, giving the setup a very clean look with no other visible equipment besides the speakers. 

An Echo Dot in the kitchen picks up music requests, and controls another Echo Dot in the AV rack connected to a [MiniDSP DDRC-24](https://www.minidsp.com/products/dirac-series/ddrc-24) running Dirac Live, which then connected to my [Rotel RB-1050](https://www.cnet.com/products/rotel-rb-1050-power-amplifier/). The Rotel was a fantastic amp, and the audio sense feature worked flawlessly. I could just stop the music and the amp would power off quickly to save power. It even handled low volume playback perfectly -- that was always an issue with my Emotiva Fusion Flex, listen too quietly and the amp would just shut off your music entirely.

Overall this piece of the system worked really well, and always sounded great. The Rotel was compact taking up 2U on the rack, and the Echo Dot + DDRC-24 hung out on top of some other components.

## Home Theater

My home theater setup is a different beast entirely, at the time consisting of:

* [Emotiva XMC-1](https://emotiva.com/products/xmc-1) - 7.2 processor with Dirac Live
* [Emotiva XPA-5 Gen1](https://www.stereophile.com/content/music-round-51) - 5 channel amplifier, 4U
* [QSC Pro USA 900](https://www.qsc.com/resource-files/productresources/amp/discontinued/usa/q_amp_usa_series_specs.pdf) - 2 channel amp, 3U
* [Two Crown XLS2500](https://www.crownaudio.com/en/products/xls-2500) - 2 channel amp, 2U each

On the speaker side of things, I had:

* [Three DIYSoundGroup HTM-12's](https://www.diysoundgroup.com/home-theater-speaker-kits/home-theater-series/home-theater-monitors/htm-12-kit.html) for the Left + Center + Right channels
* [Four DIYSoundGroup Volt-10's](https://www.diysoundgroup.com/home-theater-speaker-kits/home-theater-series/volt/volt-10-v2.html) for surround channels
* [Two home built 15" subwoofers with Dayton RSS390HF](https://www.parts-express.com/dayton-audio-rss390hf-4-15-reference-hf-subwoofer-4-ohm--295-468) drivers
* [Buttkicker LFE](https://thebuttkicker.com/buttkicker-lfe/) tactile transducer bolted to the couch

All told, that's 11 channels of amplification taking up 11U of space on my rack, for 10 items needing power. The original configuration had the LCR, and side surround speakers on the XPA-5, rear surrounds on the QSC, subwoofers on one Crown XLS 2500, and the Buttkicker on another. (For a short time, I ran one XLS 2500 bridged per 15" subwoofer -- which was definitely too much power and I killed a driver. Whoops!)


## So why upgrade?

I had a few reasons:

1. I had pre-wired the home theater for a 7.2.4 Dolby Atmos installation. Emotiva had originally promised an Atmos update for the XMC-1 [but that didn't quite pan out as expected.](https://emotiva.com/products/xmc-1-trade-in-for-xmc-2) Either way, I would need another 4 channels of amplification if/when I wanted to make the jump to Atmos.
2. I was planning on building another two subwoofers with the same Dayton drivers, bringing my total to four subwoofers. I'd need more power still, and I was running out of space on the rack. The plan was to use a Crown XLS2500 per subwoofer pair, but then how would I power the Buttkicker?
3. The Emotiva XPA-5 had an odd quirk when it powered on -- it would create an audible hiss on all the boards that would quickly dissapate after 2-3 seconds, but it was still less than ideal. 

# Enter the Crestron


While researching various amplifier options, I came across [an interesting thread on AVSForum talking about the CNAMPX-16X60](https://www.avsforum.com/threads/the-ultimate-budget-11ch-atmos-amp.2980852/). Here was an absolute monster of an amplifier with seriously impressive specs, that goes for **next to nothing** on the used market. 

The key specs are:

* Output Power (20Hz-20kHz, all channels driven): 
	* 60Wx16 @ 8 ohms
	* 90Wx16 @ 4 ohms
	* 220Wx8 bridged @ 8 ohms (NOT 4 ohm stable on bridged channels)
* Power Bandwidth: 3Hz to 50kHz, +0/-3dB
* Frequency Response: 20Hz to 20kHz, +0/-0.1dB
* Total Harmonic Distortion (THD): ≤0.03% at full power
* IHF I.M. Distortion: ≤0.01%
* SMPTE I.M. Distortion: ≤0.03%
* Dynamic Headroom: ≥2dB
* S/N Ratio: >110dB A-weighted
* [Built by ATI Amplifier Technologies](https://www.ati-amp.com/home.php) for Crestron

The flexibility of this amp is really remarkable, it can be easily configured for 8 high power channels, 16 channels, or anything in between. I would be able to replace the Emotiva XPA-5 *and* Rotel RB-1050 in my rack with just this single unit, and drive not only my whole home theater and living room speakers from it, it could drive my future Atmos speakers as well! Sweet!

## Why's it so cheap?

I definitely did my research before purchasing, and I think it boils down to this:

The Crestron amp *needs* to be part of a Crestron install to even power on. 

Normally this would be setup by the AV installer as part of a whole-home control system in a rack full of Crestron gear, programmed by the installer so the home owner can easily control anything in the house. I don't have hard numbers, but my gut feeling is most Crestron installs probably start in the 20,000 dollar range, and the sky's the limit. For a good example of the types of homes Crestron gear ends up in, the [Chalon Road home on Costal Homeworks' website](https://coastalhomeworks.com/portfolio.php) is a great example -- a CNAMPX-16X60 is even visible in the last photo of that photo gallery:

![](/media/crestron/rackwiring.jpg)

> A CNAMPX-16X60 happy at home in a proper rack full of Crestron gear, with a gorgeous wiring job by the installer. It's even labeled "Amp #3", so I would not be surprised if there's at least 2 more in the rack too.


I suspect some of the units on the used market are from houses where they redid the AV gear entirely after a few years in use, but who knows. Either way, the amplifier doesn't show any visible signs of life if you just plug a regular power cable in leading some sellers to think it might be dead and then list it cheap for parts.

The original price for the CNAMPX-16X60 is hard to find as Crestron gear is typically sold only through high end AV installers, but it's likely somewhere in the 4000+ dollar range. I managed to get mine on eBay for just *308 dollars shipped!*



# Modding the Creston CNAMPX-16X60

Since I'm not interested in investing in a whole rack of Crestron gear and tracking down the programming software, I needed a way to just power this thing on

## Cresnet explanation

Crestron gear uses a proprietary communication network to link together to a master controller called Cresnet. 

> The Cresnet® bus is the communications backbone for many Crestron® keypads, lighting controllers, shade motors, sensors, and other devices. Cresnet is a simple, yet flexible 4-wire network that provides bidirectional communication and 24VDC power for Cresnet devices.
> 
> [Source](https://www.crestron.com/Products/Interconnects,-Interfaces-Infrastructure/Infrastructure/Cresnet-Cables/CRESNET-P-BK-SP500)

On the CNAMPX-16X60 the Cresnet board is responsible controlling all of the relays in the amplifier, monitoring temperatures, and protecting the amplifier if any speaker terminals short circuit. It can even control power on individual amplifier boards to save power if only one room/zone is playing music. 

The actual protocol is a bit of a mystery to me still. I'll cover this a bit on the future enhancements section. 


## Powering on the amp

![](/media/crestron/IMG_9927.jpeg)

> Powering on the amp for the first time to test all the channels. At this point I was battery powering everything else connected to it in case anything went wrong.

To do anything with the CNAMPX-16X60 I had to power on the Cresnet board first. The official spec requires 24 volts DC power, but luckily it was perfectly happy with only 19 volts -- which was great since I have a large collection of old laptop power supplies. Once I connected power I hit the "bypass" button on the back of the amp, and heard all the relays click to life. 

Success!

But this wasn't really a final solution by any means. The bypass button is a nice workaround if you don't have a full Crestron install, but it's necessary to hit that button any time the Cresnet power source resets. While that could be as rare as the occasional power outage... leaving the amp on 24x7 would waste electricity and generate unnecessary heat. This amp idles at around 150 watts so it's not insignificant!

Ultimately, I needed a better way to control the amp's power state.

## Cracking it open

![](/media/crestron/IMG_9965.jpeg)

Let's see what's inside! The bulk of this amp is the two massive toroidal transformers upfront, and then the 8 massive amplifier boards that each power two channels. The heatsinks to cool these are massive as this amp is entirely passively cooled. 


![](/media/crestron/IMG_9977.jpeg)

On the far side we find the Cresnet board that controls everything. Given all the protection and diagnostics this board is capable of, I wanted to keep it intact as much as possible. Other users have done much more extensive modifications to this amp including replacing relays or even removing relays entirely. This is not only a more expensive and time consuming modification, but removing the relays prevents short circuit protection from kicking in and could lead to the amp frying itself.

First I focused on trying to understand how the bypass button worked. I began tracing contacts and looking for solder points. My first hunch was if I could possibly just bridge the connections for the bypass button so it's always pressed -- unfortunately this simple solution didn't work. Through experimentation I found that once the Cresnet board gets power it takes 3-5 seconds for the bypass button to activate. Pressing it any sooner or powering on Cresnet with it already pressed does nothing. Alright, so I need a timer circuit of some sort.

![](/media/crestron/IMG_9979.jpeg)

I busted out an old Arduino I wasn't using for anything else and managed to track down the following points on the board:

* Bypass button (white wire)
* 5V DC power (red wire)
* Ground (black wire)
* 5V DC power than engages when the amp is turned on, used to light a status LED (white wire that switches into blue... was running out of colors!)

With these combined it was trivial to throw together some code on the Arduino that runs any time Cresnet gets power. The code starts off just waiting for 5 seconds, then reads the current status of the amp and then "presses" the bypass button by connecting it to ground briefly. The extra check of reading the current status helps protect against false positives or unexpected behavior if an external user hits the bypass button manually first.

![](/media/crestron/IMG_9981.jpeg)

Installed, and working great! I got my sister to help me carry the amp down to the basement and load it in the AV rack. I have a beefy Panamax power-strip that has built in 12V trigger support already, so I plugged the laptop power supply into that and plugged that into the Cresnet power socket. The Emotiva XMC-1 processor has 12V trigger output, so when I turn that on or off, the amps in my stack follow suit now. 

Bing, bang, boom, 12V controlled Crestron! 

![](/media/crestron/IMG_0302.jpeg)

> It's hard to believe 4 screws hold this 90 lb beast in place just fine. Also, yes, that QSC amp has seen better days. I got it for 70 bucks on Craigslist though and it still works great.

# Modifying some XLR -> RCA cables

One of the neat things about the CNAMPX-16X60 is it can bridge the stereo pair of inputs to output more power if desired. Officially this is done with the [CNXBRMO module](https://www.crestron.com/Products/Inactive/Discontinued/A-M/CNXBRMO), and officially only half the channels can be bridged... but unofficially you can bridge the outputs with just a simple modified cable, and it works on all channels. You do need balanced outputs from your source though.

![](/media/crestron/cable.jpg)

[I used these cables from Amazon](https://www.amazon.com/gp/product/B07T3PGVNX/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1), normally intended to take a mono XLR signal and split to two RCA outputs while keeping the same phase. Bridging the amplifier channels though requires two out of phase signals, one fed to each input. Luckily this is how balanced audio works in the first place, so it's just a trivial modification to this cable. 

![](/media/crestron/IMG_9917.jpeg)

From the factory these cables have both the left + right output channels bonded to pin 2, and pins 1 and 3 are grounded together. Pin 3 would typically carry the negative signal. 

![](/media/crestron/IMG_9918.jpeg)

Some resoldering later, and now we have a cable with "left" output wired to pin 2, "right" output to pin 3, and ground remains connected to pin 1. (Sorry for the perspective flip) Rinse and repeat for the other cables!

![](/media/crestron/IMG_9957.jpeg)

> Bench testing the freshly modified balanced cable. A Focusrite Scarlett 2i2 provided the balanced output needed. 

# Argh! Ground Loop Interference!

So I had the power situation all figured out, my cables made, and the amp loaded into my rack. My first pass at wiring the amp used bridged channels for the left + center + right speakers, and regular channels for the surround speakers.

![](/media/crestron/IMG_1299.jpeg)

Alas, as soon as I powered it up... something was wrong. My old nemesis ground loop interference was rearing its ugly head, again. [We go way back.](https://www.youtube.com/watch?v=NiMxxDcQHKk) I don't quite understand all the possible sources of ground loop interference, but it's plagued me for years over many different combinations of hardware. It most often occured when using my gaming PC connected over HDMI, typically sources like an AppleTV or gaming console wouldn't cause the issue.

The balanced connections from the XMC-1 were mostly immune to this issue thankfully, though I did have issues with the XPA-5. I solved those with using a few [XLR ground pin lifters.](https://www.amazon.com/Hosa-GLT-255-Female-Ground-Stopper/dp/B00FC4YPL4)  (Some amps like those from Parasound actually have this feature built in with some optional switches!) If I recall correctly, I only needed two of these to resole the issues, despite having 5 XLR interconnects between the XMC-1 and XPA-5.

But now with the Crestron in the mix I was using the unbalanced outputs from the XMC-1 for the surround speakers, and the interference was back with a vengeance. If I fired up a game on my PC the surround channels would emit an awful screeching noise that easily over powered any real content. This simply would not do. 

![](/media/crestron/IMG_9984.jpeg)

At first I tried running a separate grounding connection between the Crestron amp and the gaming PC, which did actually help quiet down the noise, but only slightly.


![](/media/crestron/IMG_9985.jpeg)

I tried desparately connecting that ground lead to everything else in the AV rack too, hoping to strike the magical combination of grounding, but to no avail. I tried different RCA cables, swapping channels around on the amp, etc, but nothing resolved it.

Finally, I hopped in my car and sped over to [Best Buy to buy some ground loop isolators](https://www.bestbuy.com/site/metra-ground-loop-isolator-black/9855136.p?skuId=9855136) and wired them in. Things improved dramatically... but the noise was still there, now a faint whisper like a buzzing fly from a few feet away. 

But I could still hear it.

I finally just removed the surround channels from the equation, and confirmed that the left + center + right speakers were dead silent -- no interference to be had. Reluctantly I ordered 4 more XLR -> RCA cables, it seems that the balanced outputs from the XMC-1 don't end up creating the ground loops that ultimately cause all the interference. 

Once the cables arrived (and I modded them), I finally had all 7 channels working noise free. But there went my plan of having 11 channels of home theater amplification in a single chassis! (+2 for the living room.) 

Oh well. I'm still waiting on the perfect processor come out anyway with HDMI 2.1, Dirac Live, and 7.2.4 processing, so this is a problem for another day.

# Improving remote control

Alright, so everything is finally working well and sounding great. The XMC-1 controls the power to the Crestron and tells it to turn on or off as needed which is fantastic for home theater use... but less than ideal for living room use. Basically to listen to music in the living room now I had to power on almost everything for the home theater too -- Crown XLS and QSC amps included. All combined these amps consumed something like 250 to 300 watts of power just idling. I'd also forget to turn the XMC-1 off sometimes too since I can't see if it's on or off from the living room, so these amps would stay on overnight or even for a few days sometimes before I realized it. Not ideal at all! 

But it still worked, so I used the Crestron amp like this for a few months while brainstorming improvements.

![](/media/crestron/esp8266.jpg)

**Enter [the ESP8266!](https://www.amazon.com/KeeYees-Internet-Development-Wireless-Compatible/dp/B07HF44GBT/)**

A friend of mine turned me on to the little marvel that is the ESP8266, and I've since deployed a number of them around my home controlling various things like LEDs, and also modified some ESP8266 based Sonoff POW power meters to notify me when my laundry completes. This cheap little board packs an 80 Mhz processor and Wi-Fi stack and is compatible with most Arduino code making it perfect to throw together little IoT devices.

I started to plan out how I wanted this to work:

* Obviously needed a 12V trigger input to continue interfacing with all the home theater equipment
* MQTT support to tie into my existing Home Assistant platform
* Audio sensing for analog audio input (spoiler, I didn't get this far -- see future enhancements)
* Since I didn't want to remove the Arduino in the Crestron amp already, the ESP8266 ultimately needed to control the flow of 19V DC power to the Cresnet board

![](/media/crestron/IMG_2244.jpeg)

> Cat oversees beta testing of the circuit. The two LED strip sections on the left + right represent input 12V trigger, and output 19V. 

For the 12V trigger I used a basic voltage divider circuit with 100k and 33k ohm resistors to step down to around 3V to safely input to a GPIO pin on the ESP8266. I then polled in the code every second looking for 3 consecutive readings before acting.

	// Check the states of the 12 volt trigger
		if(states[TRIGGER]){
			Serial.print(" current state is on and the pin");
			if(digitalRead(TRIGGER_PIN) == LOW){
				Serial.println(" is low!");
				if (++stateChangeCount > NUM_SAMPLES){
					states[TRIGGER] = false;
					turnOffIfApplicable();
					stateChangeCount = 0;
				}
			} else{ 
				Serial.println(" is high!");
				stateChangeCount = 0;
			}
		} else {
			Serial.print(" current state is off and the pin");
			if(digitalRead(TRIGGER_PIN) == HIGH){
				Serial.println(" is high!");
				if (++stateChangeCount > NUM_SAMPLES){
					turnOn(TRIGGER);
					states[TRIGGER] = true;
					stateChangeCount = 0;
				}
			} else{ 
				Serial.println(" is low!");
				stateChangeCount = 0;
			}
		}


The MQTT part is relatively easy in the code. It both subscribes to a topic to listen for commands so it can be turned on remotely, and publishes its current state to a topic so Home Assistant knows when its on.

	void mqttCallback(char* topic, byte* payload, unsigned int length){
		Serial.println("Callback triggered!");
		
		char payload_assembled[length];
		for (int i = 0; i < length; i++) payload_assembled[i] = (char)payload[i];
			Serial.print("Payload -- ");
		Serial.println(payload_assembled);
		
		if (strcmp(payload_assembled, "ON") == 0){
			turnOn(OVERRIDE);
			Serial.println("Turning on override");
		}
		else if (strcmp(payload_assembled, "OFF") == 0){
			states[OVERRIDE] = false;
			turnOffIfApplicable();
			Serial.println("Setting override off.");
		}
	}
	
	void mqttPost(){
		Serial.println("Posting update to MQTT");
		DynamicJsonBuffer json;
		JsonObject &root = json.createObject();
		
		root["state"] = (states[CURRENT_STATE] ? "ON" : "OFF");
		root["trigger"] = (states[TRIGGER] ? "TRIGGERED" : "OFF");
		root["audio"] = (states[AUDIO] ? "AUDIOED" : "OFF");
		root["override"] = (states[OVERRIDE] ? "OVERRIDDEN" : "OFF");
		
		char buffer[root.measureLength() + 1];
		root.printTo(buffer, sizeof(buffer));
		mqtt.publish(MQTT_ATTRIBUTES_TOPIC, buffer, true);
		mqtt.publish(MQTT_STATE_TOPIC, (states[CURRENT_STATE] ? "ON" : "OFF"), true);
	}
	
	// there's some more MQTT handling code but this is the fun stuff


The last part was controlling the output of our 19V DC. At first I put together the circuit with a mosfet, having just succesfully implemented an LED controller in a [Tetris Light](https://www.amazon.com/Bitopbi-Stackable-Induction-Interlocking-Lighting/dp/B07FVTCKDG) using the same technique of an ESP8266 controlling a mosfet (though that one faded in and out very nicely -- this would just be a hard on or off.)


	void turnOn(uint8_t source){
		states[CURRENT_STATE] = true;
		states[source] = true;
		digitalWrite(RELAY_PIN, 0);
		mqttPost();
	}
	
	// only triggers a turn off if 12v trigger is off, audio sense is off, and user has not requested a manual override
	void turnOffIfApplicable(){
		mqttPost();
		if(!states[TRIGGER] && !states[AUDIO] && !states[OVERRIDE])
			turnOff();
	}
	
	void turnOff(){
		states[CURRENT_STATE] = false;
		digitalWrite(RELAY_PIN, 1);
		mqttPost();
	}


I had it all working with my LED test loads but once I moved the new circuit down to the Crestron amp in the rack and wired it up, I found the circuit starting behaving unpredictably. I'm not sure if it was more grounding issues, or some interference from the sheer number of electronics in the rack, but the 19V output would erratically turn on or off. I ended up swapping out the mosfet with a relay board from [Kookye's Smart Home Sensor kit](https://www.amazon.com/KOOKYE-Modules-Arduino-Raspberry-Professional/dp/B01J9GD3DG), and it's been working reliably ever since!

![](/media/crestron/crestron_ha.png)

Now I can remotely power on the Crestron amp from Home Assistant (and through that Amazon Alexa), and enjoy my music in the living room without having to power on all of the other power hungry home theater amplifiers. I even have an automation to shut the Crestron amp off at 3 AM too, lest I ever forget it's on.

# Future Enhancements

## Audio Sensing

Since Alexa doesn't have automation triggers for starting/stopping music playback, right now I have to either ask Alexa to turn on the Crestron amp, or pop into Home Assistant to manually turn it on. I would love to figure out how to build a circuit so the ESP8266 can listen in to music playback using an unused output from the MiniDSP DDRC-24 and automatically turn the Crestron on or off as needed. I've been told this isn't exactly easy though, so it's not high on my priority list now.

## Cresnet Serial Control

Right now the system is ostensibly still pretty dumb. Given that I have two different zones running off the Crestron amp, it will always have a section powered up that is not actually in use. If I'm in the home theater then 7 of the 8 amp boards are in use, and the extra power for the unused amp board is pretty inconsequential compared to everything else. But if I'm just using it in the living room, only 1 of the 8 amp boards is actually in use, and given that I almost always have music going, that adds up!

Since this Crestron amp is meant to be installed in large whole home audio systems, it was designed to enable individual amp boards to be be powered up or down as needed. But again... you need a Crestron master controller to do that, which I do not have nor want just for this purpose. So if I can figure out a way to connect an ESP8266 to the Cresnet IO on the Crestron Amp, and get all the commands right, I could unlock the full potential of this amp! 

Cresnet is based on the RS-485 protocol which is reasonably easy to work with. I even found [a Github project where someone put together a monitoring program to capture commands](https://github.com/StephenGenusa/CresnetMon), and it seems like there is [community interest in putting together a simulated Crestron controller.](https://www.reddit.com/r/crestron/comments/gdppx4/cresnet_protocol/) I do not know of the rammifications though if that's a potential infringement and regardless, for my purposes I do not have the means to capture the commands necessary to interface with the CNAMPX-16X60. I'll keep dreaming though!
