# Convolution Neural Network for hand-written digit recognition (MNIST dataset)
The objective of this assignemment is to rewrite the MNIST digit recognition code with following constraints
1. 99.4% validation accuracy
2. Less than 20k Parameters
3. Use any learned technique  
4. Less than 20 Epochs
5. Use Batch Normalization and Dropout
6. Optional: Use GAP layer in place of a Fully connected layer

## Model building considerations

|Sl. No.|Design consideration|Comment|
|-------|--------------------|-------|
|1.|How many layers|3 blocks are used in the models. Each block consists of 2 convolution layes. Except the last convolution block, each connvolution block is followed by a transition block|
|2.|MaxPooling|Max pooling with kernel size 2 and stride 2 is used as first layer of each transition block to reduce channel dimension.|
|3.|1x1 Convolutions|1x1 convolution (Antman kernel) is used as the 2nd layer of transition block to consolidation number of channels. This helped in reducing number of parameters in control.|
|4.|3x3 Convolutions|3x3 convolutions with padding 1 and stride 1 is used for feature extraction in layers in convolution block|
|5.|Receptive Field||
|6.|SoftMax|log_softmax is used to the feed output to NLL_loss (negative log-likelyhood loss)|
|7.|Learning Rate|Learning rate started with a value of 0.01 and then reduced couple of times using multi-step LR scheduler while approaching end of training (higher number of epochs)|
|8.|Kernels and how do we decide the number of kernels?|This is a function or cost /compute. In this case, number of total parameters. As each 3x3x1 kernel will contribute 9 parameters to be trained, number of kernels are kept low in starting in each block and then then increased to reach a certain level and then reduced again using AntMan Kernel|
|9.|Batch Normalization|Used after each Convolution layer (post ReLU)|
|10.|Image Normalization|Done as part of Image transformations while loading training and test data. Normalization is done using meand and standard deviation of complete dataset.|
|11.|Position of MaxPooling|Maxpooling used after at least 2 convolution layers and not used after last convolution block before prediction|
|12.|Concept of Transition Layers|Used in the network which helped in controling number of Kernels in each block|
|13.|Position of Transition Layer|Transition layer is used after each convolution block|
|14.|DropOut|Drop-out is used afetr each transition block, except last one. Intially started with dropout of 0.25, but that was preventing the model to be trained to required level. Thus, final value of dropout used as 0.01. Tried using dropout after each convolution layer, but that did not allow to increase training accuracy to appropriate level|
|15.|When do we introduce DropOut, or when do we know we have some overfitting|Overfitting is understood comparing loss and accuracy charts for training and testing for each epoch. No overfitting is observed afetr introducing dropout|
|16.|The distance of MaxPooling from Prediction|Maxpooling is not used after last convolution block before prediction|
|17.|The distance of Batch Normalization from Prediction|Batch Normalization is not used in last convolution layer / antman layer and GAP layer before prediction|
|18.|When do we stop convolutions and go ahead with a larger kernel or some other alternative (which we have not yet covered)||
|19.|How do we know our network is not going well, comparatively, very early|Comparing loss and accuracy for training and testing after each epoch. This helps us understand whether we need to add more capacity for training well|
|20.|Batch Size, and Effects of batch size|Tried with batch size of 64, 128, 256 and 512. In thsi case 128 faired better|

## Achived result

|Sl. No.|Constraint|Achieved - Yes / No|
|-------|----------|--------------------|
|1.|99.4% validation accuracy|Yes - 99.44%|
|2.|Less than 20k Parameters|Yes - 18,520|
|3.|Use any learned technique|See section above|  
|4.|Less than 20 Epochs|Yes|
|5.|Use Batch Normalization and Dropout|Yes|
|6.|Optional: Use GAP layer in place of a Fully connected layer|Yes|
