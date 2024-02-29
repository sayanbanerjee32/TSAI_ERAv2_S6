# Back propagation worked example
The spreadsheet provides an (toy) example of a simple Neural Network with 2 inputs, 2 targets, 1 hidden layer and 8 weights. This example helps in understanding following aspects.
1. How Neuron values at hidden layer and output layer are calculated at the time of forward pass
2. How activation functions are applied at the time of forward pass
3. How errors are calculated based on target and calculated output
4. How partial derivative for error is calculated for each of the weights in the network
5. How weights are updated after each iteration
6. What role learning rate plays in updating weights
Note that the example depicts the calculations based on only 1 set of input and target pair, however, in reality, a neural network will deal with a batch of input and target pairs.
   
## Screen shot of the Spreadsheet containing all calculations 
[Download the spreadsheet](https://github.com/sayanbanerjee32/TSAI_ERAv2_S6/blob/main/S6-Part1/Class%20_BP_File_SayanBanerjee.xlsx)  

![Alt text](Class%20BP%20File_SayanBanerjee.png?raw=true "Back Propagation calculation XLSX screen shot")  

## Under lying equations for each calculation in the following network
![Alt text](network.png?raw=true "model architecture") 
### Equations for forward pass
|Equation|Description/comment|
|--------|-------------------|
|h1 = w1*i1 + w2*i2|Hidden layer neuron 1 pre-activation|
|h2 = w3*i1 + w4*i2|Hidden layer neuron 2 pre-activation| 
|a_h1 = σ(h1) = 1/(1 + exp(-h1))|activation function sigmoid as σ(x) = 1/(1+exp(-x))|
|a_h2 = σ(h2) = 1/(1 + exp(-h2))|activation function sigmoid|	
|o1 = w5*a_h1 + w6*a_h2|Output layer neuron 1 pre-activation|
|o2 = w7*a_h1 + w8*a_h2|Output layer neuron 2 pre-activation|
|a_o1 = σ(o1) = 1/(1 + exp(-o1))|activation function sigmoid|
|a_o2 = σ(o2) =  1/(1 + exp(-o2))|activation function sigmoid|
|E1 = ½ * (t1 - a_o1)²|L2 loss for target 1|
|E2 = ½ * (t2 - a_o2)²|L2 loss for target 2|
|E_Total = E1 + E2|Total loss|

### Partial derivative of E_Total with respect to w5
|Equation|Description/comment|
|--------|-------------------|
|∂E_total/∂w5 = ∂(E1 + E2)/∂w5||
|∂E_total/∂w5 = ∂E1/∂w5|∂E2/∂w5 = 0 as E2 does not depend on w5|
|∂E_total/∂w5 = ∂E1/∂w5 = ∂E1/∂a_o1*∂a_o1/∂o1*∂o1/∂w5|chain rule|
|∂E1/∂a_o1 =  ∂(½ * (t1 - a_o1)²)/∂a_o1 = (a_01 - t1)|product rule - 2 * 1/2 (t1 -a_o1) * ( 0 - 1)|
|∂a_o1/∂o1 =  ∂(σ(o1))/∂o1 = a_o1 * (1 - a_o1)|derivative of σ(x) = σ(x) * (1-σ(x))|
|∂o1/∂w5 = a_h1|as the term _w6*a_h2_ is independent of w5|
|∂E_total/∂w5 = (a_o1 - t1) * a_o1 * (1 - a_o1) *  a_h1|Pattern suggests that the final equation only consist of input a_o1 and output a_h1 that are directly associated with w5|

### Partial derivative of E_Total with respect to other weights feeding to output layer
|Equation|Description/comment|
|--------|-------------------|
|∂E_total/∂w6 = (a_o1 - t1) * a_o1 * (1 - a_o1) *  a_h2|Similarl to ∂E_total/∂w5, this equation consists of input a_o1 and output a_h2 that are directly associated with w6 |
|∂E_total/∂w7 = (a_o2 - t2) * a_o2 * (1 - a_o2) *  a_h1|Similarl to ∂E_total/∂w5, this equation consists of input a_o2 and output a_h1 that are directly associated with w7|
|∂E_total/∂w8 = (a_o2 - t2) * a_o2 * (1 - a_o2) *  a_h2|Similarl to ∂E_total/∂w5, this equation consists of input a_o2 and output a_h2 that are directly associated with w8|

### Partial derivative of E_Total with respect to hidden layer outputs
|Equation|Description/comment|
|--------|-------------------|
|∂E1/∂a_h1 = (a_o1 - t1) * a_o1 * (1 - a_o1) * w5|∂E1/∂a_h1 = ∂E1/∂a_o1*∂a_o1/∂o1*∂o1/∂a_h1, ∂o1/∂a_h1 = w5 and other components are already solved|
|∂E2/∂a_h1 = (a_o2 - t2) * a_o2 * (1 - a_o2) * w7|Similarly, ∂E2/∂a_h1 = ∂E2/∂a_o2*∂a_o2/∂o2*∂o2/∂a_h1, ∂o2/∂a_h1 = w7 and other components are already solved|
|∂E_total/∂a_h1 = (a_o1 - t1) * a_o1 * (1 - a_o1) * w5 +  (a_o2 - t2) * a_o2 * (1 - a_o2) * w7|Adding 2 equations above for ∂E_total/∂a_h1|
|∂E_total/∂a_h2 = (a_o1 - t1) * a_o1 * (1 - a_o1) * w6 +  (a_o2 - t2) * a_o2 * (1 - a_o2) * w8|Same for ∂E_total/∂a_h2|

### Partial derivative of E_Total with respect to weights feeding to hidden layer
|Equation|Description/comment|
|--------|-------------------|
|∂E_total/∂w1 = ∂E_total/∂a_h1 * ∂a_h1/∂h1 * ∂h1/∂w1|chain rule involving using already solved components |
|∂E_total/∂w2 = ∂E_total/∂a_h1 * ∂a_h1/∂h1 * ∂h1/∂w2|chain rule involving using already solved components |
|∂E_total/∂w3 = ∂E_total/∂a_h2 * ∂a_h2/∂h2 * ∂h2/∂w3|chain rule involving using already solved components |
|∂E_total/∂w4 = ∂E_total/∂a_h2 * ∂a_h2/∂h2 * ∂h2/∂w4|chain rule involving using already solved components |

__Final equations after replacing the values in above equations__
|Equation|Description/comment|
|--------|-------------------|
|∂E_total/∂w1 = ((a_o1 - t1) * a_o1 * (1 - a_o1) * w5 +  (a_o2 - t2) * a_o2 * (1 - a_o2) * w7) * a_h1 * (1 - a_h1) * i1|∂E_total/∂a_h1 is available, ∂a_h1/∂h1 is derivative of sigmoid, ∂h1/∂w1 is i1 as w2*i2 is independent of i1|
|∂E_total/∂w2 = ((a_o1 - t1) * a_o1 * (1 - a_o1) * w5 +  (a_o2 - t2) * a_o2 * (1 - a_o2) * w7) * a_h1 * (1 - a_h1) * i2|Same pattern as above|
|∂E_total/∂w3 = ((a_o1 - t1) * a_o1 * (1 - a_o1) * w6 +  (a_o2 - t2) * a_o2 * (1 - a_o2) * w8) * a_h2 * (1 - a_h2) * i1|Same pattern as above|
|∂E_total/∂w4 = ((a_o1 - t1) * a_o1 * (1 - a_o1) * w6 +  (a_o2 - t2) * a_o2 * (1 - a_o2) * w8) * a_h2 * (1 - a_h2) * i2|Same pattern as above|

### Equation for updating weights after each iteration

`new_wi = old_wi - η * ∂E_total/∂wi`  

_where_
- new_wi is the wi after update
- old_wi is the wi from last iteration
- η is learning rate
- ∂E_total/∂wi is partial derivative of total error with respect to wi from last iteration

## Effect of different learning rates (η) on error graph
Below graphs show that higher the learning rate higher the rate of reduction in loss after each iteration.

### learning_rate = 0.1
![Alt text](eta_0_1.png?raw=true "learning_rate = 0.1")  

### learning_rate = 0.2
![Alt text](eta_0_2.png?raw=true "learning_rate = 0.2")  

### learning_rate = 0.5
![Alt text](eta_0_5.png?raw=true "learning_rate = 0.5")  

### learning_rate = 0.8
![Alt text](eta_0_8.png?raw=true "learning_rate = 0.8")  

### learning_rate = 1
![Alt text](eta_1.png?raw=true "learning_rate = 1")  

### learning_rate = 2
![Alt text](eta_2.png?raw=true "learning_rate = 2")  
