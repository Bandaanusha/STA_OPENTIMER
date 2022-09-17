# STA_OPENTIMER
## Table of Contents
[1.Introduction]()<br>
[2.Timing Graphs]()<br>

## 1. Introduction
**Timing Path** : Path between start point (flop clock pin / input ports) and end point (flop d pin / output ports).

![timing path](https://user-images.githubusercontent.com/62790565/190869147-80e07d1a-d3bd-418c-82c6-7d24b0ce7b4f.jpg)

```
Timing path  : start point     -> end point
Timing path1 : Input port (IN) -> flop d pin (D)
Timing path2 : flop clock pin  -> output port(OUT)
Timing path3 : flop clock pin  -> flop d pin (D)
Timing path4 : Input port (IN) -> flop d pin (D)
```

**Arrival Time** : Time required by signal to reach endpoint.<br>

**Required Time** : Expected time for signal to arrive.

![reqtime](https://user-images.githubusercontent.com/62790565/190869810-cc994550-fa47-4bf3-abf0-4d9dd8222ea6.png)

**Slack**
- Slack is difference between arrival time and required time.
- It is of two types. Min Salck and Max Slack.
- Min Slack (Hold Timing, Hold Slack, Hold Analysis) : The difference between actual arrival time and expected minimum require time.

![minslack](https://user-images.githubusercontent.com/62790565/190870301-a5abdd83-b851-4550-acab-19305a7c1b95.png)

- Max Slack (Setup Timing, Setup Slack, Setup Analysis) : The difference between expected maximum require time and actual arrival time.

![maxslack](https://user-images.githubusercontent.com/62790565/190870315-8c803729-945f-4d34-b844-9e611153b0d0.png)

**Types of Setup/Hold analysis**
1. reg2reg : Timing path that starts at register and ends at register.

![reg2reg](https://user-images.githubusercontent.com/62790565/190870541-9acbfe0a-8b25-48cd-9e80-d8b27bd0eda9.jpg)

2. in2reg : Timing path that starts at input port and end at a register.

![in2reg](https://user-images.githubusercontent.com/62790565/190870674-ccc51513-99d1-41b3-a476-a8c50681d78c.jpg)

3. reg2out : Timing path that starts at register and end at a output port.

![reg2out](https://user-images.githubusercontent.com/62790565/190870797-5a1c5906-2137-43e4-af82-d9605feeb9ad.jpg)

4. in2out : Timing path that starts at input port and end at output port.

![in2out](https://user-images.githubusercontent.com/62790565/190870982-72449590-e822-4832-8e41-b82c1fcf0c54.jpg)

5. Clock gating : Timing path that starts from clock and ends at and gate output.

![clock gating](https://user-images.githubusercontent.com/62790565/190871135-a7c0c791-f2ef-479d-9d3c-fa8ab44c2ee6.jpg)

6. recovery/removal : Timing path between clock of flipflop that is driving a reset pin to reset pin.

![removal](https://user-images.githubusercontent.com/62790565/190871308-dc3f6b2a-8a15-418d-a441-983e95935c34.jpg)

7. data-to-data : The requirement of a to arrive a particular point and ctrl to arrive at a different point.

![data2data](https://user-images.githubusercontent.com/62790565/190871912-c571ffd3-9a4a-4527-ac31-3de3d2cba944.jpg)

8. Latch (time borrow / time given)
 
In the following timing path flipflop borrows time from latch
 
![timegiven](https://user-images.githubusercontent.com/62790565/190871999-cea7f833-7fa6-4e4c-af22-9845aa1ea90e.jpg)
 
In the following timing path latch gives time to flipflop
 
![timeborrow](https://user-images.githubusercontent.com/62790565/190872001-96b526e4-0ca5-437f-bc17-13b2188ae385.jpg)

**Slew/transition analysis**
1. Data (min/max)

![datasl](https://user-images.githubusercontent.com/62790565/190872278-9811d77b-fa21-403b-aed5-5649061b27e5.png)

2. Clock(max/min)

![clocksl](https://user-images.githubusercontent.com/62790565/190872282-c728f6e7-a5ba-41ea-8823-44133c44f4a9.png)

**Load Analysis**
1. Fanout (max/min)
2. Capacitance (max/min)

**Clock analysis**
1. Skew : Latency difference
<br>In below figure skew is latency dfference between L1,L2,L3,L4.

![clockkskew](https://user-images.githubusercontent.com/62790565/190872368-d2d66755-b73a-4527-8b58-960d8efe83fe.png)

2. Pulse Width

![clockpulsewidth](https://user-images.githubusercontent.com/62790565/190872482-77dc94ef-aa41-4486-8d81-a6c648f1171e.png)

## Timig Graphs
**reg2reg setup analysis**
Consider the following combinational circuit

![combckt](https://user-images.githubusercontent.com/62790565/190872601-0f2c2356-27ec-4faa-8f08-3cb51473ccbe.jpg)

Converting combinational circuit to Directed Acyclic Graph (DAG)

![dag](https://user-images.githubusercontent.com/62790565/190872692-d2895bbd-56de-41bc-96f9-2feec28c3590.jpg)

- Calculating **Actual Arrival Time (AAT)**  
Time at any node where we see latest transition after first rise clock edge. (Worst if a node has multiple paths) 

![ta](https://user-images.githubusercontent.com/62790565/190873170-df5eadc8-bae7-4607-9689-217720027e1a.jpg)

- Calculating **Required Time (RAT)**
Time at any node where we expect latest transition within clock cycle. (Best if a node has multiple paths)

![rt](https://user-images.githubusercontent.com/62790565/190873209-e5cdc6fc-c9e0-42e5-bbd5-5d6be5c65fa9.jpg)

- Calculating **Slack (S)**
```
Slack = Required arrival time - Actual arrival time
```

![slack](https://user-images.githubusercontent.com/62790565/190873280-037ae107-bbe8-4afc-934e-a5172117461e.jpg)

- **Graph Based Analysis** : It takes into account the worst possible path to reach an end point.
- **Path Based Analysis** : It takes into account the actual path followed to reach an end point.

![pba1](https://user-images.githubusercontent.com/62790565/190873683-7b6efe41-4893-4007-920e-9b2e53d68ef3.jpg)

![pba2](https://user-images.githubusercontent.com/62790565/190873720-3f77f843-d58b-405a-98cf-1afaac39aeb9.jpg)

- For accurate and detailed timing analysis, DAG should be with pin node conventions

![pinnode](https://user-images.githubusercontent.com/62790565/190873846-6b6942b3-017a-4e3d-9b27-3b63d5c08a50.jpg)

Calculation of arrival time, require time and slack remains same.

![pinnoars](https://user-images.githubusercontent.com/62790565/190874098-a9c88473-d28e-438c-a443-7fbf6ed83685.jpg)

## 3. Clk-to-q delay, library setup, hold time and jitter



