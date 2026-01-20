# 'Drum Pad Triggering System' & Xylophone for MIDI Usage - 1985

This repository documents a sub-project I designed and built in December 1985 to simulate SIMMONS electronic drum pads, "on the cheap" ($20 in parts
compared to $1500). The intent was to design a physical drum pad interface that would provide digital triggers to my then-new [Colossus Computer Computer](https://github.com/rcl9/Colossus_Control_Computer) which itself would create MIDI messages for a Roland TR-707 drum machine. Overall, all goals were met and the system was deemed useful and functional.

<div style="text-align:center">
<img src="/Images/Drum Pads System.jpg" alt="" style="width:75%; height:auto;">
</div>

<div style="text-align:center">
<img src="/Images/Electronics trigger interface - Nov 1985.jpg" alt="" style="width:75%; height:auto;">
</div>

## Block Diagram & Theory of Operation

The following shows a simplistic overview of how the system was structured:

<div style="text-align:center">
<img src="/Images/Drum pads block diagram.jpg" alt="" style="width:50%; height:auto;">
</div>

Each "drum pad" was made from a Nordica cottage cheese plastic container, painted shiny black. Inside and mounted to the inner top was a small pickup microphone. Its raw signal was processed by a differential compator, amplifier and rectifier circuit. The output digital trigger was then sent to a Z80-based PIO chip which created a software interrupt in the Z80 handler assembler code. 

The interrupt handler would choose one of nine pre-determined MIDI NOTE-ON (drum #) codes, a velocity code (0= no 
sound, 140 = maximum output) and the MIDI channel number. The MIDI channel would allow multiple drum machines/synthesizers to be 
attached to the same MIDI bus, so the pads could be assigned (to say) 3 toms, 1 symbal and 1 snare drum on the drum machine, then 3 
notes on a synthesizer programmed for SIMMONS toms (a well known electronic drum sound). Or all the pads could be assigned to 
different pitches of the synthesizer set up with a tom sound, so you effectively have 9 drum toms all the same but at different pitches.

## Schematics & Operation

<div style="text-align:center">
<img src="/Schematics/Drum pad trigger processing circuit.jpg" alt="" style="width:100%; height:auto;">
</div>

The basic principle was to derive a trigger pulse from a microphone or piezo-electric crystal which causes an interrupt 
from the edge triggered Z80-PIO input port B.

Nine cloned circuits were built across 3 prototyping boards:

<div style="text-align:center">
<img src="/Images/Electronics trigger interface - Nov 1985.jpg" alt="" style="width:75%; height:auto;">
</div>

Of which this shows a close-up of a single processing circuit:

<div style="text-align:center">
<img src="/Images/Close-up of one interface - Nov 1985.jpg" alt="" style="width:50%; height:auto;">
</div>

## Research and Development

My research into various types of sensors showed that high impedance crystal microphones and piezo-electric transducers were the best 
and cheapest:

<div style="text-align:center">
<img src="/Images/Radio shack piezo transducer pickup.jpg" alt="" style="height:300px">
<img src="/Images/Blue mic closeup.jpg" alt="" style="height:300px">
</div>

(The left device is a Radio Shack 273-069 piezo transducer and the right device is a small crystal mic, the latter which was used for the drum pads)

The output of the piezo-electric transducer suffered from lack of dynamic response - hitting it hard or softly did not change the 
output. This would not do well for velocity sensing that requires a sensor that could distinguish between different strike 
velocities. Another alternative studied was a crystal microphone -- no waveform is shown for it - but it did faithfully reproduce what you hear 
and the output was related to how hard it was hit. The mic was chosen as the main pad pickup (for its ability to pick up strike 
velocity) and the piezo transducer was used for a demonstration pickup which when placed on a desk or shoe would trigger the pads 
(it is VERY sensitive but output is not related to strike velocity).

<div style="text-align:center">
<img src="/Images/Piezo-Electric cystal output - hard hit.jpg" alt="" style="width:50%; height:auto;">
</div>

<div style="text-align:center">
<img src="/Images/Piezo-Electric cystal output - crystal hit directly.jpg" alt="" style="width:50%; height:auto;">
</div>

Referring to the circuit shown above, the microphone sensor's output is differentiated by an op-amp to produce to a short spike which is then amplified and rectified by the Superdiode IC1a. This spike quickly charges the tantalum cap then slowly discharges through the 4.7k (TC = 70msec). The output 
waveform of this section in shown in the following diagrams. Note that the microphone has a faster rise time so is superior to the piezo. 
This output is compared against the 'sensitivity' voltage by IC1b, which is in a comparator configuration. When the threshold 
is crossed then the output swings to the +ve rail which is then differentiated to create a narrow spike. This is reduced to +5v 
and is sent to the Z80-PIO which is programmed to interrupt on the rising edge of the pulse. 

<div style="text-align:center">
<img src="/Images/Piezo-Electric cystal output - amplified and rectified output.jpg" alt="" style="width:50%; height:auto;">
</div>

<div style="text-align:center">
<img src="/Images/Piezo-Electric cystal output - amplified and rectified microphone output.jpg" alt="" style="width:50%; height:auto;">
</div>

After all of the research and development was complete, the pads worked perfectly - they can be tuned so very sensitive that air currents will 
trigger each voice!

A follow-up project was a xylophone-style trigger interface device built from a child's toy and using the small, blue microphones as had been used in the drum pads:

<div style="text-align:center">
<img src="/Images/Xylophone interface 2.jpg" alt="" style="width:75%; height:auto;">
</div>

<div style="text-align:center">
<img src="/Images/Xylophone interface.jpg" alt="" style="width:75%; height:auto;">
</div>
