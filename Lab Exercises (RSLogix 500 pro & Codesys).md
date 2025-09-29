# Lab Exercises

I have done a few PLC programming exercises for some extra practice and decided to learn how to do the same programming on the application called Codesys.
This supposed to be a more modern way to program PLCs so learning this will be valuable.

I will show all exercises in both RSLogix 500 pro as well as codesys for now on.

## Exercise 1 (RSLogix 500 pro)

Create a program that counts to 10 seconds and turns on B3:0/0 when your PLC gets to 10.

<img width="1103" height="292" alt="image" src="https://github.com/user-attachments/assets/4e803479-b2c7-44b9-9d26-96561e88f12b" />

	• We start the program with an XIO (I:0/0) just to start the timer (T4:0, PRE = 10s).
	• Once the timer DN bit is enabled then bit (B3:0/0) turns on.

## Exercise 2 (RSLogix 500 pro)

Create a program that counts to 10 in 2 second intervals (e.g. every 2 seconds, count up by one). When you get to 10, turn on B3:0/1.

<img width="1199" height="473" alt="image" src="https://github.com/user-attachments/assets/003d453e-8301-40d6-adbc-64d9d58275c6" />

	• We start with a XIO (B3:0/1) to check to see if the bit (B3:0/1) is active. If it is not then that starts out timer (T4:0, PRE = 2s)
	• Once our timer (T4:0) DN bit is active, then our counter (C5:0, PRE = 10) adds 1 to the counter and resets our timer.
	• The timer and counter will go into a loop until the counter has reached 10, then that will turn on our bit (B3:0/1)
	•  Because the bit is active the loop stops.
  
## Exercise 3 (RSLogix 500 pro)

Create a program that causes B3:0/2 to blink on and off at 2 second intervals (on for 2 seconds, off for 2 seconds). The logic should run continuously until the PLC is turned off.

<img width="1235" height="430" alt="image" src="https://github.com/user-attachments/assets/f3995f58-91a8-4a25-9817-9d4af65adc48" />

	• We start with XIO (T4:1/DN) to check and see if timer 2 (T4:1) DN bit is active. If not then it starts timer 1(T4:0, PRE = 2s).
	• Once timer 1 DN bit is active then that starts timer 2. Once timer 2 DN bit is active then timer 1 is no longer active.
	• Because timer 1 needs timer 2 DN bit to be inactive and timer 2 needs timer 1 DN bit to be active this creates a ping pong between the 2 timers.
	• The timers bounce to each other every two seconds in a continuous loop.
	• The lamp (B3:0/2) uses this ping pong effect and turns on and off in 2 second intervals.

## Exercise 4 (RSLogix 500 pro)

Create a program that blinks B3:0/3 on and off at 2 second intervals 10 times (B3:0/3 should turn on 10 times).

See codesys version below......

## Exercise 5 (RSLogix 500 pro)

Create a program that requires a single bit (B3:0/0) to be turned on for 4 seconds before a second bit (B3:0/2) is turned on. The second bit should remain on, even if B3:0/0 is turned off, until B3:0/1 is turned on and then off.

See codesys version below......

## Exercise 6 (RSLogix 500 pro)

Create a program that correctly accounts for any sensor bounce on input I:0.0 on PLC 141.225.164.170. Be able to correctly count 4 drops of the paddle/bar.

<img width="1287" height="438" alt="image" src="https://github.com/user-attachments/assets/5698e75c-214e-4586-9df5-f75d9967f08c" />

	• We start with a XIC (I:0/8) as our sensor input. When the sensor is triggered then it starts the debounce timer.
	• The debounce timer (T4:0, PRE = 0.5s) is there to filter out noise that the sensor picks up randomly.
	• Once the debounce timer DN bit is active then it adds one to the counter (C5:0, PRE = 4).
	• The counter reaches 4 when the sensor triggers on and off four times and gets passed the debounce timer "filter".
	• We can reset the counter by hooking the reset function on a physical switch like I did on the last rung.

## Exercise 7 (RSLogix 500 pro)

Create a program on PLC 141.225.160.139, that includes anti-tie-down logic, where both buttons must be pressed for 4 seconds before turning on B3:0/0.

<img width="1366" height="642" alt="image" src="https://github.com/user-attachments/assets/6cd384df-4b2e-49fd-ac48-93de6cde7149" />
<img width="1377" height="363" alt="image" src="https://github.com/user-attachments/assets/0d4a9913-0342-4df9-a113-671f28ae0f8d" />

	• 90% of this ladder logic is mainly for the Anti-Tie Down buttons.
	• We start with the first two rungs that has both the Anti_Tie Down left and right buttons (respectfully) and has a timer (T4:0 & T4:1, both PRE = 0.5s).
	• The third rung consists of two XIC for both of the Anti-Tie Down buttons & XIO for both of the DN bit of the two timers and an Anti-Tie Down OK coil.
	• This works like this: Both buttons needs to be pressed at the around the same time (half a second of each other) to work.
	• The third rung insures that if a button was pressed and the other was pressed after the half a second then because one of the timer DN bit is active, the XIO instruction is no longer true.
	• Once both buttons are pressed at the same time then the OTL (Logic Start) bit becomes latched and the 4 second timer starts (T4:2, PRE = 4s).
	• Once that timer DN is active then bit (B3:0/0) turns on.
	• The last rung basically says that if any of the buttons were no longer pressed then the logic start latch becomes unlached and you need to unpress the remaining button to start over again.

## Exercise 1 (Codesys)

Ok so codesys works a bit different than RSLogix, but the ladder logic is basically the same. 
There are times where I need to do a bit of the logic differently, but overall the logic follows the same ast the RSLogix counter part.
The biggest different is the inputs and outputs uses variables instead of addresses.

Create a program that counts to 10 seconds and turns on B3:0/0 when your PLC gets to 10.

<img width="951" height="461" alt="image" src="https://github.com/user-attachments/assets/4b130b0e-cf13-4f9f-ae1d-7c7d58437749" />

	• We start the program with an XIO (Start) just to start the timer (PT = 10s).
	• Once the timer Q bit is enabled then the Lamp turns on.

## Exercise 2 (Codesys)

Create a program that counts to 10 in 2 second intervals (e.g. every 2 seconds, count up by one). When you get to 10, turn on B3:0/1.
  
<img width="949" height="530" alt="image" src="https://github.com/user-attachments/assets/ef9b09f2-1533-44fb-a9b4-7aa0b4a58e8c" />

	• We start with a XIO (Lamp) to check to see if the Lamp is active. If it is not then that starts out timer (PV = 2s).
	• Once our timer Q bit is active, then our counter (PV = 10) adds 1 to the counter.
	• The timer and counter will go into a loop until the counter has reached 10, then that will turn on our Lamp
	•  Because the Lamp is active the loop stops.
	•  Codesys does not have a reset function, but you can reset the timers by turning the IN bit off. 
	•  To do so I added a XIO (Timer.Q) input on the first rung (rung is called network on codesys) so that once the timer Q bit is active it reset the timer.

## Exercise 3 (Codesys)

Create a program that causes B3:0/2 to blink on and off at 2 second intervals (on for 2 seconds, off for 2 seconds). The logic should run continuously until the PLC is turned off.

<img width="951" height="557" alt="image" src="https://github.com/user-attachments/assets/2556e7df-e4aa-45c5-9902-cf2a68847a3f" />

	• We start with XIO (Timer_2.Q) to check and see if timer 2 Q bit is active. If not then it starts timer 1(PT = 2s).
	• Once timer 1 Q bit is active then that starts timer 2. Once timer 2 Q bit is active then timer 1 is no longer active.
	• Because timer 1 needs timer 2 Q bit to be inactive and timer 2 needs timer 1 Q bit to be active this creates a ping pong between the 2 timers.
	• The timers bounce to each other every two seconds in a continuous loop.
	• The lamp uses this ping pong effect and turns on and off in 2 second intervals.

## Exercise 4 (Codesys)

Create a program that blinks B3:0/3 on and off at 2 second intervals 10 times (B3:0/3 should turn on 10 times).

<img width="952" height="700" alt="image" src="https://github.com/user-attachments/assets/5b5f9c6e-829b-4f1d-abec-b50c17272c3c" />

	• We start with XIO (Timer_2.Q) so that it starts timer 1 (PT = 2s) when timer 2 (PT = 2s) Q bit is inactive.
	• When timer 1 Q is active it turns on the Lamp and adds one to the counter (PV = 10) and the timer ping pong loop continues until the counter reaches 10.
	• The Lamp turns off when timer 2 becomes active, so that the lamp turns on and off on 2 second intervals.
  • Once the counter reaches 10 then the counter Q bit turns on the Lamp and stops the loop.

## Exercise 5 (Codesys)

Create a program that requires a single bit (B3:0/0) to be turned on for 4 seconds before a second bit (B3:0/2) is turned on. The second bit should remain on, even if B3:0/0 is turned off, until B3:0/1 is turned on and then off.
   
<img width="953" height="593" alt="image" src="https://github.com/user-attachments/assets/9f08774c-23bd-40d9-b808-ebf376c1c7b1" />

	• We start our logic with XIO (Bit_1) checking to see if the first bit is active, if not then its starts the timer (PT = 4s).
	• As the timer is counting (Timer.IN) is true and turns Bit_0 on.
	• Once the timer Q bit is active it goes through the XIO (Bit_1) because the bit is off and turns on Bit_2 on.
	• Once Bit_2 is on it seal in that last rung so its always active even if Bit_0 is not.
	• To reset everything Bit_1 needs to be active and deavtivated again.

## Exercise 6 (Codesys)

Create a program that correctly accounts for any sensor bounce on input I:0.0 on PLC 141.225.164.170. Be able to correctly count 4 drops of the paddle/bar.

<img width="951" height="514" alt="image" src="https://github.com/user-attachments/assets/0daac776-28cb-434d-b454-3d7737a1ee1d" />

	• We start with a XIC (Sensor) as our sensor input. When the sensor is triggered then it starts the debounce timer.
	• The debounce timer (PT = 0.5s) is there to filter out noise that the sensor picks up randomly.
	• Once the debounce timer Q bit is active then it adds one to the counter (PV = 4).
	• The counter reaches 4 when the sensor triggers on and off four times and gets passed the debounce timer "filter" and this turns on the Lamp.
	• We can reset the counter by hooking the reset function on a physical switch like I did on the last rung.

## Exercise 7 (Codesys)

Create a program on PLC 141.225.160.139, that includes anti-tie-down logic, where both buttons must be pressed for 4 seconds before turning on B3:0/0.

<img width="948" height="873" alt="image" src="https://github.com/user-attachments/assets/28395ec5-7967-4c41-bae5-4bc6cdbdd4cc" />

	• 90% of this ladder logic is mainly for the Anti-Tie Down buttons.
	• We start with the first two networks that has both the Anti_Tie Down left and right buttons (ATD_1 & ATD_2 respectfully) and has a timer (ATD_Timer1 & ATD_Timer2, both PT = 0.5s).
	• The third network consists of two XIC for both of the Anti-Tie Down buttons & XIO for both of the Q bit of the two timers and an Anti-Tie Down OK coil (ATD_OK).
	• This works like this: Both buttons needs to be pressed at the around the same time (half a second of each other) to work.
	• The third rung insures that if a button was pressed and the other was pressed after the half a second then because one of the timer Q bit is active, the XIO instruction is no longer true.
	• Once both buttons are pressed at the same time then the coil (PRG_Start) bit becomes true and the 4 second timer starts (PT = 4s).
	• Once that timer Q is active then the Lamp turns on.
	• The last Network basically says that if any of the buttons were no longer pressed then the PRG_Start becomes unlached and you need to unpress the remaining button to start over again.
