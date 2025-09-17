# Debounce & Traffic Lights

## Debounce

**Objective:**

To allow the student to become more knowledgeable with programming controllers by designing an automatic control system.

**The Process:**

Count the number of times that the prox sensor is made - simulates product coming down a conveyor and hitting a hard stop.

**Details:**

Use PLC 164.170 to count the number of times a "box" has been seen. When the count reaches 5, turn on light1. Use input 2 to reset the counter (and turn off the light).

<img width="1161" height="409" alt="image" src="https://github.com/user-attachments/assets/79e604d9-cc44-4f78-a778-a52d3be1aadf" />

**Program Explanation:**

	• The sensor (I:0/8) waits until it detects an object by proximity.
	• The TON timer (T4:0) is placed to account for debounce, so that if anything trips the sensor too quickly the counter does not count.
	• Once a "box" is detected by the sensor, and the TON Timer (T4:0) is active for half a second, the counter (C5:0) increments.
	• The cycle repeats, Sensor > TON Timer > Counter, until the counter reaches 5  then the DN bit is active.
	• The DN bit activates the Light (O:0/0) which turns it on.
	• We can manually reset the counter by using input 2 (I:0/1) and flipping the switch, which also turns off the light since the DN bit of the counter will deactivate.






# Traffic Lights

**Objective:**

Program a PLC to behave as a 4-way stop traffic light (North and South side, and East and West)

<img width="1161" height="497" alt="image" src="https://github.com/user-attachments/assets/1f8cdf7e-588b-44c5-a0a4-982c6cb6dcb7" />
<img width="1161" height="549" alt="image" src="https://github.com/user-attachments/assets/e23a4c38-e15b-4326-a928-715612067ec1" />

**Program Explanation:**

	• The program starts by checking if there is a 0 in the B3:0/0 bit and if that is the case then OTL output puts a 1 in the B3:0/0 bit.
	• The next input checks to see if the B3:0/0 bit has a 1 and if it does then it starts the NS_Green timer (T4:0)
	• While the NS_Green timer is timing the green light is active, because of the two inputs (XIC & XIO) XIC (T4:0/EN) & XIO (T4:0/DN) turns the green light on because the EN bit is on passing through XIC input and the DN bit is off passing through the XIO bit. 
	• Once the NS_Green timer's PRE = ACCUM then the DN bit is active turning off the green light and start the NS_Yellow timer.
	• The same logic applies to the NS_Yellow, EW_Green, and EW_Yellow timer and light.
	• After the green and yellow timers and lights both red lights check to see if the green or yellow lights are off, if they are the red lights are on. 
	• While the North & South side lights are going through the cycle, the East and West red light is active and vice versa. 
	• After both cycles are complete, the EW_Yellow timer DN bit unlatches (OTU) the B3:0/0 and the last input checks to see if the B3:0/0 bit is 0, if so then the NS_Green timer resets and the cycle loops.

