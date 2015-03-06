[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**ANALYSIS**](Analysis) > [**KINEMATICS**](Kinematics)

To generate a kinematics plot, first create a ```TKinematics``` object.  A constructor is available that uses char arrays to call ```TNucleus``` objects.  To use it, the syntax is as follows:

```c++
TKinematics(beam_energy,beam,target,ejectile,recoil)
```
As an example, 
```c++
 TKinematics *mykin = new TKinematics(520.0,"Sr95","p","p","Sr95")
```

Next, if we want to generate the E vs Lab Theta we would use the function ```Evslab```.  The syntax for that is:
```c++
Evslab(start_angle,stop_angle,angle_step_size,particle)
```
The particle field needs a bit of explanation, it is an integer corresponding to what one you are interested in, from right to left in the TKinematics constructor.  As an example:

```c++
 mykin->Evslab(1,89,1,2)
```
Where the ```2``` means we are interested in the proton.  This returns a TSpline3 object, which can be drawn or stored using the typical ROOT commands.
