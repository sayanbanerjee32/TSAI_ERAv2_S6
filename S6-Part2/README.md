# Convolution Neural Network for hand-written digit recognition (MNIST dataset)
The objective of this assignment is to rewrite the MNIST digit recognition code with following constraints
1. 99.4% validation accuracy
2. Less than 20k Parameters
3. Use any learned technique  
4. Less than 20 Epochs
5. Use Batch Normalization and Dropout
6. Optional: Use GAP layer in place of a Fully connected layer

[Go to the S6 notebook](https://github.com/sayanbanerjee32/TSAI_ERAv2_S6/blob/main/S6-Part2/ERAv2_S6_P2_SayanBanerjee_v6.ipynb)  


## Model building considerations

|Sl. No.|Design consideration|Comment|
|-------|--------------------|-------|
|1.|How many layers|3 blocks are used in the models. Each block consists of 2 convolution layers. Except the last convolution block, each convolution block is followed by a transition block|
|2.|MaxPooling|Max pooling with kernel size 2 and stride 2 is used as first layer of each transition block to reduce channel dimension.|
|3.|1x1 Convolutions|1x1 convolution (Antman kernel) is used as the 2nd layer of transition block to consolidation number of channels. This helped in reducing number of parameters in control.|
|4.|3x3 Convolutions|3x3 convolutions with padding 1 and stride 1 is used for feature extraction in layers in convolution block|
|5.|Receptive Field|Effective receptive field for the network is 38x38|
|6.|SoftMax|log_softmax is used to the feed output to NLL_loss (negative log-likelihood loss)|
|7.|Learning Rate|Learning rate started with a value of 0.01 and then reduced couple of times using multi-step LR scheduler while approaching end of training (higher number of epochs)|
|8.|Kernels and how do we decide the number of kernels?|This is a function or cost /compute. In this case, number of total parameters. As each 3x3x1 kernel will contribute 9 parameters to be trained, number of kernels are kept low in starting in each block and then then increased to reach a certain level and then reduced again using AntMan Kernel|
|9.|Batch Normalization|Used after each Convolution layer (post ReLU)|
|10.|Image Normalization|Done as part of Image transformations while loading training and test data. Normalization is done using mean and standard deviation of complete dataset.|
|11.|Position of MaxPooling|Maxpooling used after at least 2 convolution layers and not used after last convolution block before prediction|
|12.|Concept of Transition Layers|Used in the network which helped in controlling number of Kernels in each block|
|13.|Position of Transition Layer|Transition layer is used after each convolution block|
|14.|DropOut|Drop-out is used after each transition block, except last one. Initially started with dropout of 0.25, but that was preventing the model to be trained to required level. Thus, final value of dropout used as 0.01. Tried using dropout after each convolution layer, but that did not allow to increase training accuracy to appropriate level|
|15.|When do we introduce DropOut, or when do we know we have some overfitting|Overfitting is understood comparing loss and accuracy charts for training and testing for each epoch. No overfitting is observed after introducing dropout|
|16.|The distance of MaxPooling from Prediction|Maxpooling is not used after last convolution block before prediction|
|17.|The distance of Batch Normalization from Prediction|Batch Normalization is not used in last convolution layer / antman layer and GAP layer before prediction|
|18.|When do we stop convolutions and go ahead with a larger kernel or some other alternative (which we have not yet covered)|Not considered|
|19.|How do we know our network is not going well, comparatively, very early|Comparing loss and accuracy for training and testing after each epoch. This helps us understand whether we need to add more capacity for training well|
|20.|Batch Size, and Effects of batch size|Tried with batch size of 64, 128, 256 and 512. In this case, 128 fared better|

## Achieved result

|Sl. No.|Constraint|Achieved - Yes / No|
|-------|----------|--------------------|
|1.|99.4% validation accuracy|Yes - 99.44%|
|2.|Less than 20k Parameters|Yes - 18,520|
|3.|Use any learned technique|See section above|  
|4.|Less than 20 Epochs|Yes|
|5.|Use Batch Normalization and Dropout|Yes|
|6.|Optional: Use GAP layer in place of a Fully connected layer|Yes|

# Training and testing methodology
## Network architecture

Below is the list of layers starting from input to output.
|Sl. No.|Layer|Additional details|
|-------|-----|------------------|
|1.|3x3 convolution with padding 1, stride 1, bias false|input size: 28 x 28 x 1, output size: 28 x 28 x 8, receptive field: 3|
|2.|Batch normalization||
|3.|3x3 convolution with padding 1, stride 1, bias false|input size: 28 x 28 x 8, output size: 28 x 28 x 16, receptive field: 3 + (3-1) * 1 = 5|
|4.|Batch normalization||
|5.|3x3 convolution with padding 1, stride 1, bias false|input size: 28 x 28 x 16, output size: 28 x 28 x 32, receptive field: 5 + (3-1) * 1 = 7|
|6.|Batch normalization||
|7.|2x2 max pooling with stride 2|input size: 28 x 28 x 32, output size: 14 x 14 x 32, receptive field: 7 + (3-1) * 1 = 8|
|8.|1x1 convolution with padding 1, stride 1, bias false|input size: 14 x 14 x 32, output size: 14 x 14 x 8, receptive field: 8 + (1-1)*2 = 8|
|9.|Batch normalization||
|10.|Dropout|| dropout probability 0.1|
|11.|3x3 convolution with padding 1, stride 1, bias false|input size: 14 x 14 x 8, output size: 14 x 14 x 16, receptive field: 8 + (3-1) * 2 = 12|
|12.|Batch normalization||
|13.|3x3 convolution with padding 1, stride 1, bias false|input size: 14 x 14 x 16, output size: 14 x 14 x 32, receptive field: 12 + (3-1) * 2 = 16|
|14.|Batch normalization||
|15.|2x2 max pooling with stride 2|input size: 14 x 14 x 32, output size: 7 x 7 x 32, receptive field: 16 + (2-1) * 2 =18|
|16.|1x1 convolution with padding 1, stride 1, bias false|input size: 7 x 7 x 32, output size: 7 x 7 x 8, receptive field: 18 + (1-1) * 4 =18|
|17.|Batch normalization||
|18.|Dropout|| dropout probability 0.1|
|19.|3x3 convolution with padding 1, stride 1, bias false|input size: 7 x 7 x 8, output size: 7 x 7 x 16, receptive field: 18 + (3-1) * 4 = 26|
|20.|Batch normalization||
|21.|3x3 convolution with padding 1, stride 1, bias false|input size: 7 x 7 x 16, output size: 7 x 7 x 32, receptive field: 26 + (3-1) * 4 = 34|
|22.|Batch normalization||
|23.|2x2 max pooling with stride 2|input size: 7 x 7 x 32, output size: 3 x 3 x 32, receptive field: 34 + (2-1) * 4 = 38|
|24.|1x1 convolution with padding 1, stride 1, bias false|input size: 3 x 3 x 32, output size: 3 x 3 x 10, receptive field: 38 + (1-1) * 8 = 38|
|25.|Global Average Pooling|input size: 3 x 3 x 10, output size: 1 x 1 x 10 the squeeze to array of 10|

_Refer to the below image for number parameters in each layer_
![Alt text](model_arch.png?raw=true "model architecture") 

## Training and testing steps
Following are the steps that are performed in the code
1. Data
   - Download of MNIST dataset
   - Training set - Image augmentation and Image normalization and then train data loader creation
   - Test set - Image normalization and then test data loader creation
2. Training - For each epoch
   - Data is split in batch of 128 image and target pair using train data loader.
   - Each batch of images passed (forward pass) through the network to get predictions.
   - Loss is calculated using negative log-likelihood function based on correct target vector and predicted class vector(log-SoftMax of model output)
   - Loss is backpropagated 
   - Weights are updated after each batch based on learning rate multiplied by partial derivative of loss with respect to weight
3. Learning rate configurations
   - learning rate started with 0.01
   - Momentum of 0.9 is used to overcome local minima issues
   - Multistep LR scheduler is used to reduce the learning rate by 1/10th after 10 and 17 epochs to ensure slower step in gradient descent in higher epochs
4. Testing - and the end of each epoch
   - Data is split in batch of 128 image and target pair using test data loader.
   - Each batch of images passed (forward pass) through the network to get predictions.
   - Average Loss is calculated using negative log-likelihood function based on correct target vector and predicted class vector(log-SoftMax of model output)
   - Test loss and accuracies are logged after each epoch
6. Visualising training performance
   - the train and test losses and accuracies are plotted to understand whether there is overfitting

### Comparison of training and test loss and accuracies

As per the below charts as test / validation loss is always less then train loss and test / validation accuracy is always higher than train accuracy, there is no case of overfitting observed in the training process.
![Alt text](train_test_loss.png?raw=true "model architecture")
