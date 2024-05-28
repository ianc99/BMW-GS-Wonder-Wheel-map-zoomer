<div class="WordSection1">

**<span lang="EN-IE" style="mso-ansi-language:EN-IE">Objective </span>**

<span style="font-family:&quot;Segoe UI&quot;,sans-serif;color:#1F2328;
background:white">Create a device capable of sniffing the CAN Bus of a
2020 BMW R1250 GS motorcycle and translating the messages from the
Wonder Wheel into something which could be translated into Zoom,
<span class="GramE">Centre,<span style="mso-spacerun:yes"> 
</span>reorientate</span> etc on a standard Android phone or
tablet.<span style="mso-spacerun:yes">  </span>This will be accomplished
using an ESP32 based <span class="SpellE">NodeMCU</span> and CAN Bus
adapter. </span>

<span style="font-family:&quot;Segoe UI&quot;,sans-serif;color:#1F2328;
background:white">There are commercial products which can do this – but
I wasn’t aware of them when I started and anyway the primary aim of this
project was to learn a little about CAN-Bus and have a bit of soldering
fun.<span style="mso-spacerun:yes">  </span>Much of the code is borrowed
from others and I’m afraid what little I did write is
<span class="GramE">pretty poor</span>.<span style="mso-spacerun:yes">  
</span></span>

**<span style="font-family:&quot;Segoe UI&quot;,sans-serif;
color:#1F2328;background:white">Components </span>**

<span style="font-family:&quot;Segoe UI&quot;,sans-serif;color:#1F2328;
background:white">I’m connecting to the exact items I used, but there
are many options possible for both the EPS board and the MCP based
CAN-bus adapter.<span style="mso-spacerun:yes">  </span>If I build
another of <span class="GramE">these</span> I’ll probably opt for
smaller footprint <span class="SpellE">irems</span>.</span>

[<span class="SpellE">Hailege</span> 2pcs MCP2515 CAN Bus Module TJA1050
Receiver SPI Module for Arduino <span class="GramE">AVR :</span>
Amazon.co.uk: Business, Industry &
Science](https://www.amazon.co.uk/dp/B07Z1V2RTM?psc=1&ref=ppx_yo2ov_dt_b_product_details)

<span style="mso-no-proof:yes"><img src="Wonderwheel_files/image001.png" data-border="0"
v:shapes="_x0000_i1028" width="286" height="226"
alt="A blue circuit board with yellow and blue objects Description automatically generated" /></span>

[<span class="SpellE">AZDelivery</span> 3 x ESP32-DevKitC
<span class="SpellE">NodeMCU</span> <span class="SpellE">WiFi</span>
WLAN CP2102 ESP32-WROOM-32D IoT 2-In-1 Microcontroller Bluetooth Module
Development Board compatible with Arduino including
E-Book<span class="GramE">! :</span> Amazon.co.uk: Electronics &
Photo](https://www.amazon.co.uk/dp/B074RGW2VQ?psc=1&ref=ppx_yo2ov_dt_b_product_details)

<span style="mso-no-proof:yes"><img src="Wonderwheel_files/image003.png" data-border="0"
v:shapes="_x0000_i1027" width="346" height="179"
alt="A close-up of a circuit board Description automatically generated" /></span>

[1/5/10/30 sets kit TE <span class="SpellE">tyco</span> AMP 4 pin male
female waterproof Audi Oxygen Sensor AV Video Audio Plug BMW auto
connector 1-967640-1
(aliexpress.com)](https://www.aliexpress.com/item/33035334981.html)<span style="mso-spacerun:yes"> 
</span>(Not yet arrived)

I also had an old 12v-\>USB power adapter which put out 5v @3a
regulated.<span style="mso-spacerun:yes">  </span>I can’t see the exact
ones online anymore – but you’ll need something for the voltage
stepdown.

 

**Hardware Assembly**

Disclaimer – I’ve only assembled this in test mode and connected to a
laptop (for diagnostics and 5v power) so far, but I’ve got it
functioning end to end.

<span style="mso-no-proof:yes"><img src="Wonderwheel_files/image005.png" data-border="0"
v:shapes="_x0000_i1026" width="602" height="363"
alt="A computer on a blue towel Description automatically generated" /></span>

 

<span class="SpellE">**NodeMCU**</span> **\<-\> TJA1050 connections**

|  |  |  |
|----|----|----|
| <span class="SpellE">NodeMCU</span> | TJA1050 | Comment |
| GPIO25 | INT |   |
| GPIO26 | SCK | <span class="SpellE">Clk</span> pin |
| GPIO27 | SI | AKA MOSI |
| GPIO14 | SO | You need a 4k7 resistor inline here because the ESP32 pins on the <span class="SpellE">NodeMCU</span> only take 3.3v and the MCP2515 on the TJA1050 puts out 5v.<span style="mso-spacerun:yes">  </span>The “proper” way to do this is with a CD4050 See here [CD4050](https://www.build-electronic-circuits.com/4000-series-integrated-circuits/ic-4050/). |
| GPIO12 | CS | Chip Select |
| GND | GND |   |
| 5V | VCC |   |

 

**Bike \<-\> Voltage regulator \<-\> <span class="SpellE">NodeMCU</span>
+TJA1050 connections.**

To connect to the <span class="GramE">bike</span> I plan to make a
“passthrough” harness with a tee off for the 4 wires to go to my
board(s) utilising a male and female version of the plugs mentioned
above from
<span class="SpellE">Aliexpress</span>.<span style="mso-spacerun:yes"> 
</span>Pinouts shown below.

<span style="mso-no-proof:yes"><img src="Wonderwheel_files/image006.png" data-border="0"
v:shapes="Picture_x0020_1" width="264" height="202"
alt="A hand holding a black device Description automatically generated" /></span>

Connecting Bike \<-\> Volt Regulator \<-\>
<span class="SpellE">NodeMCU</span> + TJ1050

<span style="font-family:Symbol;mso-fareast-font-family:Symbol;mso-bidi-font-family:
Symbol"><span style="mso-list:Ignore">·<span style="font:7.0pt &quot;Times New Roman&quot;">        
</span></span></span>Connect the CAN Hi & Lo from the bike to the Hi &
Lo on the TJ1050

<span style="font-family:Symbol;mso-fareast-font-family:Symbol;mso-bidi-font-family:
Symbol"><span style="mso-list:Ignore">·<span style="font:7.0pt &quot;Times New Roman&quot;">        
</span></span></span>Connect power from Switched 12V to the
<span class="SpellE">NodeMCU</span> & TJ1050 via voltage regulator.

I disconnected the CAN-Bus 4 wire plug from the ABS module under the
passenger saddle and put my harness inline and reconnected.

 

**<span style="font-family:&quot;Segoe UI&quot;,sans-serif;
color:#1F2328;background:white">CAN-Bus messages</span>**

<span style="font-size:12.0pt;font-family:&quot;Segoe UI&quot;,sans-serif;
mso-fareast-font-family:&quot;Times New Roman&quot;;color:#1F2328;mso-font-kerning:0pt;
mso-ligatures:none;mso-fareast-language:EN-GB">A group of people have
been working on reverse engineering the BMW motorcycle CAN bus messages
in
a </span><span style="color:black;mso-color-alt:windowtext">[<span style="font-size:12.0pt;font-family:&quot;Segoe UI&quot;,sans-serif;mso-fareast-font-family:
&quot;Times New Roman&quot;;mso-font-kerning:0pt;mso-ligatures:none;mso-fareast-language:
EN-GB">Google
Spreadsheet</span>](https://docs.google.com/spreadsheets/d/1tUrOES5fQZa92Robr6uP8v2dzQDq9ohHjUiTU3isqdc/edit#gid=0)</span><span style="font-size:12.0pt;
font-family:&quot;Segoe UI&quot;,sans-serif;mso-fareast-font-family:&quot;Times New Roman&quot;;
color:#1F2328;mso-font-kerning:0pt;mso-ligatures:none;mso-fareast-language:
EN-GB">. I've used this as a reference for my parsing.</span>

**<span style="font-family:&quot;Segoe UI&quot;,sans-serif;
color:#1F2328;background:white">Software</span>**
The software is here src="code/CAN_Read_plus_Keyboard_V0.4.ino"
 

 

<span style="font-family:&quot;Segoe UI&quot;,sans-serif;color:#1F2328;
background:white"></span>

 

<span style="font-family:&quot;Segoe UI&quot;,sans-serif;color:#1F2328;
background:white"></span>

 

<span lang="EN-IE" style="mso-ansi-language:EN-IE"></span>

 

<span lang="EN-IE" style="mso-ansi-language:EN-IE"></span>

 

</div>

