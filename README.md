# Latches and Flipflop
In this project we will be analyzing the working of different types of latches and then making a flipflop out of the preferable one.We will be using LTSpice as a simulator and tsmc180 library for this purpose.

# What is a latch?
A latch is circuit that can either hold a data or become transparent to input depending on the state of clock.In a typical level sensitive latch,it passes the input to output when the clock is HIGH,any change in input will be reflected at output when the clock is HIGH.When the clock gets LOW,the latch holds the latest data and will be insensitive to changes in input.

# Design 1
We try to implement a very basic form of a latch in this design.We just use the concept of pass transisor to achieve the transparency.The corresponding design can be seen below.


![](design/des1.PNG)

The basic idea is,when clk is HIGH,output Q will be transparent to D and when clk is LOW,it should hold the latest value.But as we no NMOS pass weak-1,so output will be less than Vdd by one threshold voltage.So the output is not completely transparent to D.Moreover,when the clock goes LOW,the output node is floating,it discharges quickly and is unable to hold the value.We witness this behaviour according to the output obtained below.

![](ouput/des1op.PNG)

Thus the above circuits have lots of demerits as:
1. It is not completely transparent to input.
2. It cannot hold the data when latch is inacive.

Considering such demerits,we look for a new design that can perform according to expectation.

# Design 2
In this design we try to address the issues of the previous design.
- In the previous design,one of the issues was that latch is not completely transparent.It was passing weak-1 or weak-0 depending on the type of transistor we use.We resolve this issue by using a transmission gate that uses both NMOS and PMOS in parallel and so would be getting both strong-0 ans strong-1.
- Another issue was,that the circuit was not able to hold the data when the latch is inactive.To solve this issue,we use two inverters back to back as they will be forming a feeback kind of thing and we would be able to hold the output.

The design so proposed,is realised as shown below.

![](design/design2.PNG)

In this design,instead of providing input,D direcly to diffusion input of the transmission gate,we use an inverter to buffer the input.If we apply input directly todiffusion input of transmission gate,there may be noise,so we use an inverter.When the clock is HIGH,the feedback path is broken by the OFF transmission gate and the latch is transparent.When clock is LOW,the input is disconnected by the OFF transmission gate and the feedback path is complete that holds the data.This design will work as desired.The latch is transparent to input when clock is HIGH and opaque when clock is LOW.The output so obtained is shown below.

![](ouput/design2op.PNG)

So we see that we are able to achieve the required functionality of latch.This design requires 10 transistors.If we further wish o reduce the number of transistors,we may try out the design described below.

# Design 3
If we wish to reduce the number of transistors,we may remove the transmission gate on the feedback path and try observing the behaviour.The modified circuit is shown below.

![](design/des3.PNG)

The corresponding output we get is shown below:

![](ouput/des3op.PNG)

If we look into the output,we find the output is not as per expectation.This can be explained by the scenario as below.
- When D is HIGH,the node X is LOW.Now suppose at HIGH value of clock,D becomes LOW,so at node X there is a fight between the PMOS transistor of inverter I1 trying to pull X up and the NMOS of I3,trying to pull it down.If the inverter I1 is not stronger enough than the I3 inverter,then node X will not change according to input and we may get an invalid output.So for getiing proper output we make the inverter I1 stronger than I3.We resize them as: 
    - PMOS-> W1=1400n and W3=180n
    - NMOS-> W1=700n and W3=180n

The output so obtained is shown as:

![](ouput/des3op2.PNG)

Thus we see that now the output is as per expectation.We acheived this by using 8 transistors.Though we reduced the number of transistors,it has some drawbacks as:
- We have to increase the size of inverter by a significant amount to make it stronger.This may create a problem,by increasing the parasitic capacitances and thus leading to increased power dissipation.
- Also if the inverter-inverter feedback goes to a point of metastability where we cannot define proper output as it lies in an intermediate value,then we have to wait for noise to come and move the feedback circuit out of metastable condition and take it to a valid output.


Considering all such demerits,we will be considering the Design 2 for realising our flipflop circuit.

# Flipflop
Flipflop is a circuit,that passes the input only during rising or falling edge of clock.We design a flipflop by using two complementary latches one after another.If the first latch is transparent when clock is HIGH,then the second latch will be transparent when clock is LOW.So suppose we want to make negative edge sensitive flipflop,we make the first latch transparent when clock is HIGH and second latch transparent when clock is LOW.Thus the first latch will be passing the date to the intermediate node when clock is HIGH.Just when the clock falls,the input gets disconnected and the second latch becomes active and it captures the latest input at the time of negative transition and holds it untill next negative edge of clock.So such a circuit is realized and shown below.

![](design/des4.PNG)

The circuit we realized is a negative edge triggered flipflop.The latch we used is same as that discussed in Design 2.The output obtained for this circuit is shown below.

![](ouput/des4op.PNG)

We can see that output waveform meets expectation.The number of transistors used to realize the Flipflop is 18.

# Conclusion
The purpose of this project was just to design and analyze various types of D-latches and make a D-flipflop using one of the designs used for latch.Although we discuessed three ways of desiging the latches,there varous other ways to design a latch.
