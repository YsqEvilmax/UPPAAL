------------------------------------------------------------------------------------
UPPAAL--TrainControl
------------------------------------------------------------------------------------

Author: Shawn syu702


1.Specification
There are many (in this model, 3) parallel railways which have sensor to determine a train is to approach or leave (several trains may approach or leave together).
Once any sensor detects a train approaches, informing the light to turn red and then close the gate (turn red first and then close).
Once there is no train on the railways, let the gate open and then turn the light to green (Open first and then turn green).
Each train will take 15 - 30s to approach and leave.
The gate will take 5s to open and close; the ligth will take 2s to change.

2.Modeling
The model is consisted of four parts, namely, Trace, Sensor, Light and Gate.

Trace: 
There are a set of "Trace" (from 1 to N). Initially, trains are all "Faraway". Each of them can receive a train from "Faraway " to "Appr" (take 15 to 30s). After the train has crossed the traffic road, let it "Leave" to "Faraway" (take 15 to 30s).

Sensor:
Accordingly, each "Trace" has a "Sensor" to send signals to "Light" and "Gate". When detecting that a train comes, a "Sensor" increases "trainCount" and sends "turnRed" signal to light. Once finding that the train has left, a "Sensor" decreases "trainCount". If there is no train on all "Trace", let the "Sensor" send a "rise" signal to "Gate"; otherwise, do nothing at all.

Light:
At the beginning, "Light" is "Red". If "Light" receives "turnRed" at "Red" location, it just stays "Red"; or if "Red" reveives "turnGreen", it will take 2s to turn to "Green". The process at "Green" is conversed but similar. 
It is worth noting that "Red" should send a "lower" signal to "Gate" as long as there is no train on "Trace".

Gate:
At the initial stage, the "Gate" is "Closed". Once "Gate" reveive a "rise" signal, it will take 5s to turn to "Open"; when a "lower" signal is reveived at "Open" location, the "Gate" will take 5s to change to "Closed". If "Gate" is "Open" and there is no train on "Trace", the "Gate" will send a "turnGreen" signal to "Light".

3.Verification
a.Trace
The trace file named TrainControl.xtr storage the posible satuation this state machine could perform.
At the beginning, each of the trace comes a train. Then the sensors detect their approaching, sending approaching siganl to light. However, the light is red right now so it just stays red.

Then, trains will cross the road as soon as posible since the state of cross is urgent, otherwise trains will stay at cross as long as they want.

Next, one of the trains is leaving and this is detected by the sensor. Considering there are still other trains approaching or crossing, the sensor just transformed to next state without sending any signal.

Until the last train has left and the signal was detected by the sensor, it will signal the gate to rise up.Since there could be train coming at any moment, the signal pair of rise and lower should be urgent and be sent as soon as possible.

Before the gate has been completely open, there is a train approaching. The gate just keep openning; however, it will go closing as soon as it is open.   

After this train has left, the gate will open agin. This time there is no train coming and the gate is successfully open. If there is no train coming, the gate will send a signal to let light turn green. Since there could be train coming at any moment, the signal pair of turn red and green should be urgent and sent as soon as possible.

b.Property
The verification of required properties has been done in the model with comments. All of them have been satisfied and there is no deadlock in the model.
