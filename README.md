# TECH-3822 Lab #1 Micrologix
On any of the Micrologix 1100's, create a program such that:

	• when B3:0.0 is enabled, B3:0.6 turns on
	• when B3:0.1. is not enabled, B3:0.7 turns on
	• when B3:0.2 and B3:0.3 are enabled, B3:0.8 turns on
	• when B3:0.4 or B3:0.5 are enabled, B3:0.9 turns on
For the above, all four activities can be programmed into a single program that will look like the following.

<img width="849" height="474" alt="image" src="https://github.com/user-attachments/assets/f11ab04e-48ae-4d9f-b2e0-108a30924624" />

## Blinking Light

Create a program that causes a light to blink on any of the PLCs that have light stacks attached to them. The solution should work such that as soon as you download it, and place the PLC in run mode, it will begin blinking the light. The light should be on for 1 second and then off for 1 second.

<img width="1650" height="656" alt="image" src="https://github.com/user-attachments/assets/13c2abf6-cf92-4434-ad50-deace3a6ef63" />


	• TON timers are set to 1 second each.
	• XIO is set to check and activate when TON Timer 2 (T4:1) is done (DN is true)
	• XIC is set to check when TON Timer 1 (T4:0) is done (DN is true)
	• When Timer 1 is done the DN bit turns on (That starts timer 2 and turns on the LED)
	• When timer 2 finishes then timer 1 starts over again (Which turns off the LED repeating the cycle)


