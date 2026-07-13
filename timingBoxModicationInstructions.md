# Modifying Timing boxes  
Due to a series of miscommunications with the manufactures or the manufacturers not using they're brain when offering their design service

Unplug the headers of the FTDI cable from the PCB.  
Loosen the outer retaining nut of the cable strain relief gland using a 19mm spanner and remove it from the shell of the timing box.  
Feed the DuPont style header through the strain relief gland. This is easiest done by feeding single headers one at a time until the rest can fit through the gland.   
The new 5V cable has the part number C232HD-**E**DHSP. The other cable ending **D**DHSP operates at 3.3V  
  
The old momentary power switch can be freed from the case of the timing box by loosening the outer retaining nut using a 14mm spanner. And then desoldered with a chisel tipped soldering iron. Tin the lobes on the new switch and then solder the leads back on. The geniuses that assembled the boxes have soldered the leads directly to the   
  
The PCB also needs to be de-adhered from its backing. IPA seems to work to soften the glue around the 4 mounting holes and then remove using a small flat head screwdriver. Not a lot of force is needed but it is possible to knock components if you’re too heavy handed.  
  
Flashing the firmware is achieved using the micro-usb port on the Pico. Anyone without a serious brain injury will note that there isn’t a lot of room to get a usb cable in and you’d be right - there was supposed to be a separate cutout for the usb port but European Circuits ignored that part of the design document. I was never able to find a decent source for low profile micro usb cables but you can cannibalise most usb cables to remove the over-moulding around the micro-usb connector, then the metallic shielding around the connector, to allow us to bend it in just the right way to let the FTDI cable and the micro-usb cable to co-exist.   
  
Here is an example of the over moulding removed revealing the metallic shield  

<img width="480" height="640" alt="IMG_4499 Medium" src="https://github.com/user-attachments/assets/f53b49e0-d118-48ae-99b9-9e6de9b936aa" />

A pair of side-cutter and needle-nose pliers lets you peel off the metallic shell to reveal the inner conductors of the cable  
  
When flashing firmware be very gentle when unplugging and re-plugging the usb cable. Movement will fatigue the solder joints between the USB connector and the cable to failure so be gentle.  
  
With the USB cable in place, the new FTDI cable can be fitted. First feed the headers through the threaded cap, and then the toothed part of the strain relief gland - don’t tighten the threaded cap yet. Like when removing the old FTDI cable this is best done one header at a time - all ten will not fit at once. Feed the headers through the threaded nut and tighten this. Leave the threaded cap for now - this is what locks the cable in place. Plugging in the headers is best done with the GND (black) pin first then the RXD (yellow), TXD (orange), and VCC (red) pins. The other pins aren’t connected to anything so are only plugged in for tidiness sake. The labels on the PCB match the FTDI diagram - RXD maps to RXD and TXD maps to TXD etc.  
Setting the strain relief is achieved by feeding through enough of the black cable such that the thinner gauge coloured wires aren’t pulling, or causing strain, on the header. Once you’re happy with this NOW you can tighten the thread cap -this is what actually locks the cable in place! 
<img width="841" height="541" alt="C232HD UART Cable connection and Mechanical Details" src="https://github.com/user-attachments/assets/9bbd10cc-aead-4127-8821-f461bd499d90" />

Now just flash the firmware (see separate doc) and re-attached the lid using the 4 Phillips / pozi-drive screws.   
  
If all went well it should look something like this:

<img width="480" height="640" alt="IMG_4502 Medium" src="https://github.com/user-attachments/assets/6da1b865-70e2-481f-ad20-66afdb62cf1d" />




