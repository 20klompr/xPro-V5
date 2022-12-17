The second example is not using a twisted pair or resistors. You are accurate, the twist does improve the signal quality, but not nearly the amount the isolator does (difference is illustrated between image 2 and image 3 above). Also note, even with the degraded signal quality (image 2),  I was still able to communicate with the spindle. 

***NOTE: The resistors were minimally effective  in reducing noise with cable lengths less than 1-meter long. (resistors help eliminate transmission line reflections that may degrade the signal; typically prominent with longer cable lengths***

Ultimately, the Huanyang VFD's due to the nature of how they are constructed, share a common ground and can unintentionally create ground loops thus permitting radiated noise to degrade the rs485 signal despite all best efforts.
![image](https://user-images.githubusercontent.com/8650709/186806336-f848f047-929f-4ca1-a04a-7f634ee90376.png)
Ultimately to mitigate this scenario I reccomend implementing an RS485 isolator as shown above.

I will post a much more extensive write-up this weekend illustrating the effects of each wiring scheme along with actual pictures showing the configuration of each.

### Spindle Types

Spindle classes are defined in the firmware (the default firmware on the xProV5 is **PWM**). The Spindle Type is dynamically selected by entering ```$Spindle/Type=XXXXX```' in the command line. There are two classes of Spindles with two separate Spindle Types.

**For the PWM and Laser Spindle classes:**
```
$Spindle/Type=PWM
$Spindle/Type=LASER 
```
**For the RS485 VFD classes:**
```