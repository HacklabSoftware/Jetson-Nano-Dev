Power supply issue

```
I was also having the same problem, in my case, I was using a 5V 5A power supply.

With no load, the power supply has 5.2v with a 0.200V decay, when I turn on the card everything works properly without over current or low voltage warnings.

But when I test the detectnet program with the camera on, the warnings of overcurrent or voltage come up.

I did a stress analysis. I tested other sources, and I identified that from the Barrel jack connector to the bottom of the TPS25944L the Efuse has an RDSon of 0.042mOhm which in total gives a voltage drop of about 200 to 400mV. and if we have an exact 5V supply when we use all the resources of the board the voltage drops to 4.6V which makes the protection act, and the kernel notification is presented.

I made some changes to the source, and I changed the voltage output to 5.3V with that ended the notifications, when I use the detecnet program the voltage of the source drops to 4.9v. which is still within the range of the board.

I would advise using 5.5v 3A sources, or greater than 3A. The TPS25944L accepts this voltage level and the other dc/dc and regulators as well.

I hope this helps, others who also have this same problem.
```
