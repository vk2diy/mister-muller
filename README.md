# Mister Muller

Mister Muller is a desktop CNC mill from gifted and found components.

A friend of mine, Eric, gifted me his unfinished project of a 24V CNC mill.

## Issues

Issues with the project were as follows.

 * Blown 24V spindle motor
 * Unsuitable lubricant (grease) applied to mechanical ways
 * No control electronics
 * Antiquated power supply

## Resolution

### Breakdown and rebuild

First I disassembled the machine and cleaned the various mechanical components.

After that I reassembled with a modern lubricant (WD40) and new control electronics ([FYSETC E4](https://www.aliexpress.com/item/1005005536751245.html) @ AUD$35).

### Assembly

Annoyingly, the board was designed for JST XH standard plugs as input and my motors did not have ends on their lines. Therefore it was necessary to buy some XH lines and solder intermediate connections with heat shrink.

I found an existing AC220V to DC24V power supply capable of outputting 5A which should be more than enough to run the system.

Once that was done I realised the control board was designed for 3D printers and did not have an obvious way to control a 24V DC spindle.

However, it did have some outputs apparently intended for 3D printer bed heating.

I monitored these on the scope but obtained a strange pattern.

![image](24v-outputs-before-gcode.png)

It seems these were turning on and off rapidly perhaps due to 50Hz interference from the power lines or some similar EMI effect. While this could be controller, it did not add a lot of confidence in the capacity to obtain full PWM control on the spindle using the existing codebase.

First of all I looked for a GCode command that would allow me to turn this on fully and determine whether it was a 12V output (per back of board silkscreen label) or a 24V output (per input and [apparent but not quite matching schematic](https://github.com/FYSETC/FYSETC-E4/blob/main/hardware/FYSETC%20E4_V1.0%20SCH.pdf)).

I couldn't find one, but managed to get them to turn on fully by issuing the command `M140 S999` which instructs the bed to heat up to an unobtainable temperature of 999 degrees celsius. 

![image](24v-outputs-after-gcode.png)

This indeed worked and verified at least that we were getting 24V out and that there is the capacity to switch it on these outputs baked in to the control board, which is the big problem. The remnant issue is software which we should be able to hack.

