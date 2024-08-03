# Tiny VGG on FER2013Plus 
The models explored here draws heavy influence from the models first introduced by the Visual Geometry Group from the University of Oxford, and adapted to fit the use case(Facial Expression Recognition on FER2013Plus).

Original paper  : https://arxiv.org/pdf/1409.1556  
Dataset         : https://www.kaggle.com/datasets/subhaditya/fer2013plus

## Data
Images are upscaled to 64 x 64 and transformations were applied to the data set to ensure consistency across the entire dataset.
The data set is also noted to have extremely uneven distribution between classes. Hence, weighted random sampling is used to give each class a much more even representation.

## Architecture
Here 3 different models of varying depths were implemented and tested. In the original paper, the models introduced were meant for the ImageNet Challenge 2014, which featured more classes, higher image resolutions and a larger dataset than our facial expression classification task. Hence, the models implemented will be scaled down versions of the ones originally introduced (VGG11 - VGG-19), begining with 6 convolutional layer in 3 blocks and 2 fully connected layer(instead of 3). The models are also modified with the addition of batch normalization layers after every cconvolutional layer to help improve training stability and perforamce, while retaining the characteristics that define VGG models; max pooling with 2x2 window and stride of 2, high model depth, ReLu hidden layers and the use of small 3x3 convolution filters.

The details of the impleted models are as follows;

### 3 Convolutional layer blocks 
Contains 8 Weight layers, 2 x 3 convolutional layers and 2 fully connected layers

- block 1
    - Conv(1, 32, kernel_size=3, padding=1) -> Batch Normalization - > ReLu
    - Conv(32, 32, kernel_size=3, padding=1)-> Batch Normalization - > ReLu
    - MaxPool
- block 2
    - Conv(32, 64, kernel_size=3, padding=1) -> Batch Normalization - > ReLu
    - Conv(64, 64, kernel_size=3, padding=1)-> Batch Normalization - > ReLu
    - MaxPool
- block 3
    - Conv(64, 128, kernel_size=3, padding=1) -> Batch Normalization - > ReLu
    - Conv(128, 128, kernel_size=3, padding=1)-> Batch Normalization - > ReLu
    - MaxPool
- fully connected layers
    - Linear(128 * 8 * 8, 512) -> ReLu
    - Dropout(0.5)
    - Linear(512, 8)

### 4 Convolutional layer blocks 
Contains 10 weight layers, 2 x 4 convolutional layers and 2 fully conncetd layers 

- block 1
    - Conv(1, 32, kernel_size=3, padding=1) -> Batch Normalization - > ReLu
    - Conv(32, 32, kernel_size=3, padding=1)-> Batch Normalization - > ReLu
    - MaxPool
- block 2
    - Conv(32, 64, kernel_size=3, padding=1) -> Batch Normalization - > ReLu
    - Conv(64, 64, kernel_size=3, padding=1)-> Batch Normalization - > ReLu
    - MaxPool
- block 3
    - Conv(64, 128, kernel_size=3, padding=1) -> Batch Normalization - > ReLu
    - Conv(128, 128, kernel_size=3, padding=1)-> Batch Normalization - > ReLu
    - MaxPool
- block 4
    - Conv(128, 256, kernel_size=3, padding=1) -> Batch Normalization - > ReLu
    - Conv(256, 256, kernel_size=3, padding=1)-> Batch Normalization - > ReLu
    - MaxPool
- fully connected layers
    - Linear(256 * 4 * 4, 512) -> ReLu
    - Dropout(0.5)
    - Linear(512, 8)

### 5 Convolutional layer blocks 
Contains 12 weighted layers, 2 x 5 convolutional layers and 2 fully connected layers 

- block 1
    - Conv(1, 32, kernel_size=3, padding=1) -> Batch Normalization - > ReLu
    - Conv(32, 32, kernel_size=3, padding=1)-> Batch Normalization - > ReLu
    - MaxPool
- block 2
    - Conv(32, 64, kernel_size=3, padding=1) -> Batch Normalization - > ReLu
    - Conv(64, 64, kernel_size=3, padding=1)-> Batch Normalization - > ReLu
    - MaxPool
- block 3
    - Conv(64, 128, kernel_size=3, padding=1) -> Batch Normalization - > ReLu
    - Conv(128, 128, kernel_size=3, padding=1)-> Batch Normalization - > ReLu
    - MaxPool
- block 4
    - Conv(128, 256, kernel_size=3, padding=1) -> Batch Normalization - > ReLu
    - Conv(256, 256, kernel_size=3, padding=1)-> Batch Normalization - > ReLu
    - MaxPool
- block 5
    - Conv(256, 512, kernel_size=3, padding=1) -> Batch Normalization - > ReLu
    - Conv(512, 512, kernel_size=3, padding=1)-> Batch Normalization - > ReLu
    - MaxPool
- fully connected layers
    - Linear(512 * 2 * 2, 512) -> ReLu
    - Dropout(0.5)
    - Linear(512, 8)

## Training & Tuning
The model with 5 convolutional layer blocks performed worse than both the 3 and 4 block models by around 14% at the end of the 20th epoch. While both the 3 and 4 block model performed similarly, the performance of the 4 block model was noticably more stable with less flunctuations between epcohs.

After hyper-paramter tuning with the 4 block model,  
optimal hyper-parameters:
- weight decay/ L2 reg : 0.001 
- Learning rate : 0.001
- Batch size    : 64/128
- optimizer     : Adam
- loss function : Cross-entroph loss
- Epochs        : up to 30

## Conclusion
The 4 block model with a batch size of 128 achieved an accuracy of 76.55% and 74.35% with batch size of 64 at the end of 30 epochs. The performance of the model with batch size 64 however is alot more stable, with less fluctuations between epochs.  
