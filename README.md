# STA_OPENTIMER
## Table of Contents
[1. Introduction]()<br>
[2. Timing Graphs]()<br>
[3. Clk-to-q delay, library setup, hold time and jitter]()<br>
[4. Textual Timing Reports and Hold Analysis]()<br>
[5. On-chip  Variation]()<br>
[6. OCV timing and pessimism removal]()<br>

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

![setupcondn](https://user-images.githubusercontent.com/62790565/190903087-c2cd0a38-b729-4752-99ab-8dae94edbff9.png)

The above figure gives the condition for setup time. And skew is given by 
```
skew = |Δ1 - Δ2|
```

- To know more about setup and hold time we need to look at interior circuitry of capture and launch flops. The mux level implementation of capture flop is given in the below figure.

![muximpl](https://user-images.githubusercontent.com/62790565/190904234-4e715c0b-167a-4953-853b-1e3448ed4838.jpg)

**Negative Latch** 
 
![negative latch](https://user-images.githubusercontent.com/62790565/190904280-273a24a4-010b-4667-b094-22943b890348.jpg)

- Transisitor level implementation of negative latch

![nlcd](https://user-images.githubusercontent.com/62790565/190904322-92fc2306-20be-43bf-8110-dabd2636428c.jpg)

- Working of negative latch

![worknl](https://user-images.githubusercontent.com/62790565/190904449-611ab825-1392-47ff-be5b-90e462b2e9d9.png)

**Positive latch**
- Transistor level circuit of positive latch

![plcd](https://user-images.githubusercontent.com/62790565/190904511-62702ba0-10cb-41cb-b0a5-66df021697f3.jpg)

- Working of positive latch

![workpl](https://user-images.githubusercontent.com/62790565/190904569-7473cc53-9ec2-45d5-a965-b797ae4f65b9.png)

**Positive edge triggered flip-flop using master-slave configuration**

![masslav](https://user-images.githubusercontent.com/62790565/190904671-7538e4a6-4575-494e-ad44-23577e43500c.png)

- For negative edge of clock transmisssion gate Tr1 is on and input D is latched to output Qm of negative latch. D reaches from input of negative latch to input of positive latch.
- For negative edge of clock transmission gate Tr3 is on. Inv4, Inv6 holds the Q state of slave positive latch. Also D is ready at the output of Inv5 to propagate till Q when CLK becomes high.

![nedgcllfp](https://user-images.githubusercontent.com/62790565/190905114-331521f3-b0dc-4849-adcd-4aa7d45c8ef1.png)

- **Setup Time** is the time before rising edge of CLK, that input D becomes valid i.e D input has to be stable such that Qm is sent out, to Q reliably. Input D takes atleast 3 inverter delays+ 1 transmission gate delay to become stable before rising edge of CLK. Atleast this much amount of time is needed by flipflop to accept the data. If the data comes in between this time frame the data will get corrupted. Therefore a certain amount of library setup time is needed for the data to be stable at the interiors of the flop.

- For positive edge of clock transmission gates Tr2 and Tr4 are on. Input Qm is latched to output Q of negative latch through Tr4 and Inv6. Inv2, Inv3 hold the Qm state of master negative latch. 

![pedgclfp](https://user-images.githubusercontent.com/62790565/190906930-c5abb066-fe10-4f06-845a-ee33c0194c4d.png)

- **Clock to Q delay** is the time needed to propagate 'Qm' to 'Q'. (D was stable till output of Inv5). Therefore time required, to propogate is 1 transmission gate delay + 1 inverter delay.
- **Hold Time** is the time for which D input remain valid after clock edge. In this case Tr1 is off after rising CLK. So, D is allowed to change or can change immediately after rise CLK edge. So Hold time is zero.

```
Setup Time = 3 Inverter delay + 1 Transmission gate delay
Clk-Q = 1 transmission gate delay + 1 inverter delay
Hold Time = zero
```

![analcond](https://user-images.githubusercontent.com/62790565/190908371-0aa0ef3a-b3bf-4213-aebe-f680b9330710.png)

### Generating jitter values
Two versions of clock signal

![clock version](https://user-images.githubusercontent.com/62790565/190910697-bc8ca7d5-a562-4b80-959e-ea360b0dfa51.png)

**Eye diagram**
 Combined version of two version of clock signal is called eye diagram.
 - Ideal eye diagram
 
 ![idealeye](https://user-images.githubusercontent.com/62790565/190910843-7e9bdc3a-b385-4e7c-9e27-41521a279f39.png)
 
 - Realistic eye diagram
 
 ![eyecn](https://user-images.githubusercontent.com/62790565/190910919-04ed28b5-7d4d-4f7a-ba82-7cba109e744f.png)

The clock does not appear at 0ns at every flipflop. It is bound to arrive at different time. This consideration gives one step better realistic/practical eye diagram.

- Eye diagram considering power variations
<br>Another important consideration to be taken is the power rails. In the above cases it is assumed that the power is constant and ideal which is not practical. Power Supply variations cause voltage drop and ground bounce.

![morerealeye](https://user-images.githubusercontent.com/62790565/190911031-2bd7819b-f1f5-43b6-bb55-6361682cda89.png)

- **Noise Margin**
Noise margin is amount of distortion allowed

![noise margin](https://user-images.githubusercontent.com/62790565/190911122-7f75d175-c2a4-410c-a6da-f8ae13174b69.png)

- **Jitter**
The window where the distortion is observed is called jitter. This has to be accounted for in the clock period as well. The new clock period can be 0±Δ to T±Δ.
<br>The non-corrupted data is observed in region outside which the distortion. If the data is launched or captured in this window it is considered as reliable data

![jitter](https://user-images.githubusercontent.com/62790565/190911227-9c1115d6-b855-4af3-b3da-dda0952d97e4.png)

Jitter has to be acccounted in the timing reports by parameter called Uncertainty.

![jitta](https://user-images.githubusercontent.com/62790565/190911469-a21e511a-8e1b-4fc2-b938-79def3110f70.png)

```
Data Arrival time    Data Required time
        Θ+Δ1      <     T+Δ2 - S - SU

Where,
S is Slack
SU is Uncertainity 
```

## 4. Textual Timing Reports and Hold Analysis
** Textual representation of setup analysis **

![texrep](https://user-images.githubusercontent.com/62790565/190911739-f00e726d-864c-4982-981d-53f759f5b5e1.png)

### Single clock - reg2reg Hold (min) analysis
Hold analysis is mostly dependent on launch flop. It is the time required to launch the data. In this analysis only the edge of the clock is considered not the whole period. In case of setup time the whole period was considered.

![holan2](https://user-images.githubusercontent.com/62790565/190911980-75eca38e-805e-44b6-a77a-16717f5618e3.png)

<br> ** Hold timing analysis with uncertainity **
The uncertainity caused because of the jitter has less impact in hold analysis because we consider only the launch edge of the clk, incase of setup analysis we consider both the launch edge and capture edge of the flop hence increasing the jitter thereby increasing the uncertainity. Hence incase of hold analysis we will have a lower uncertainty value.

![holdan3](https://user-images.githubusercontent.com/62790565/190912097-ab0993a1-5e38-4f5e-9db5-3c3add9f60b6.png)

**Hold analysis textual representation**

![holdantex](https://user-images.githubusercontent.com/62790565/190912169-6b8f0449-2a17-438d-95b1-cbc62058da0e.png)


## 5. On-chip  Variation
**Single Inverter** 

![inverter](https://user-images.githubusercontent.com/62790565/190912586-99480fc0-7112-41b0-b512-426e0bc6fd8a.jpg)

**Inverter chain**

![inverter chain](https://user-images.githubusercontent.com/62790565/190912882-69143411-b32a-4356-8973-fc8fb7c4bcb2.jpg)

#### Sources of variation - etching
Masks are non uniform in reality. So there are width and length variations.

![actual mask](https://user-images.githubusercontent.com/62790565/190913005-e8218781-cd76-4090-953e-ea3ffbebad2b.jpg)

![effect on inverter chain](https://user-images.githubusercontent.com/62790565/190913089-eb31fa2e-4b31-44f9-9c94-46158bad5132.jpg)

- Actual mask results in unproper etching and width and length of the transistor is non uniform across the transistor. Drain current varies with changes of width and length of the transistor.

![current](https://user-images.githubusercontent.com/62790565/190913437-4d0c00d3-aa06-4ca4-9b60-bb7b4e41a06d.jpg)

#### Sources of Varation - oxide thickness
- In oxidation process practically oxide is non-uniformly grown.

![ox1](https://user-images.githubusercontent.com/62790565/190914075-ad85e435-6f54-46db-9b33-7c20dcba7b04.jpg)

- Non uniform oxide thickness effects drain current.

![ox2](https://user-images.githubusercontent.com/62790565/190914174-9f0a6b3a-1136-4739-82c5-91ce96074a78.jpg)

#### Relationship between resistance, drain current and delay
![effe1](https://user-images.githubusercontent.com/62790565/190914540-d60f2670-5de8-49af-bf4b-c0034d5767fc.jpg)

![effe2](https://user-images.githubusercontent.com/62790565/190914547-bb6a55f6-d03d-42a7-89a8-4a8c8076fce0.jpg)

![effe3](https://user-images.githubusercontent.com/62790565/190914551-8a092426-e99b-4232-b0a9-ec6ce8d0a4bb.jpg)

![effe4](https://user-images.githubusercontent.com/62790565/190914558-af2e8317-4820-415b-bfdb-9cfe2edfc3dc.jpg)

![effe5](https://user-images.githubusercontent.com/62790565/190914562-418a92c1-c13c-4c6b-bd21-3111ff07bd02.jpg)

![effe6](https://user-images.githubusercontent.com/62790565/190914580-9bb9ac92-6af3-4563-9725-b55e9c891bc7.jpg)

## 6. OCV timing and pessimism removal
