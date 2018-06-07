# A (Yet Another) PyTorch (0.3.0) Implementation of Residual Networks

This is a [PyTorch](http://pytorch.org/) implementation of the
Residual Network architecture with basic blocks as described
paper [Deep Residual Learning for Image Recognition](https://arxiv.org/abs/1512.03385)
by Kaiming He, Xiangyu Zhang, Shaoqing Ren and Jian Sun.
This implementation gets a CIFAR10 test accuracy of %92-93 percent
when 18 layers are used (in the paper they also achieve %93 percent with 18 layers) 
Different annealing schemes or changing the order of the batchnorm and relu does not 
seem to make any significant improvements but this requires more testing to make sure. 
SGD with a decreasing learning rate and weight decay=0.0005 seems to perform the
best among all optimizers. Applying random rotations, flips and crops to the training set
at the beginning of each cycle to boost the variety in training set seems to improve
the accuracy by about %1-2 percent. ZCA data whitening and layer-wise properties 
(setting the weight decays for bias layers to 0) are also implemented.

# Architectural details

The network contains an initial convolution + 3 stages of residual blocks. 
The number of residual blocks inside each stage as well as the initial width 
of the first block can be given as inputs to the network constructer. The width 
then increases as 3 -> width -> 2* width -> 4* width -> 8* width. Convolution 
shortcut connections as well as usual addition are used as the "identity" mappings. 

# Implementation details

The code has been implemented for pytorch 0.3. So if you want to run it in 
pytorch 0.4 you need to change the way some scalars are handled (in particular
change .sum() to .sum().item() and .data[0] to .item()). 


# CIFAR-10 Results for various settings 

### Implementation of ResNet18 with step-wise learning rate
![](images/step-nozca.png)

Best test accuracy: 0.93210, training accuracy: 0.99820 (cost:0.01380)

| Class  | Score                                       |
|--------|---------------------------------------------|
| plane  | Precision:0.93, Recall: 0.93, F1 norm: 0.93 | 
| car    | Precision:0.96, Recall: 0.96, F1 norm: 0.96 | 
| bird   | Precision:0.92, Recall: 0.90, F1 norm: 0.91 | 
| cat    | Precision:0.86, Recall: 0.87, F1 norm: 0.87 | 
| deer   | Precision:0.92, Recall: 0.95, F1 norm: 0.94 | 
| dog    | Precision:0.90, Recall: 0.89, F1 norm: 0.90 | 
| frog   | Precision:0.96, Recall: 0.95, F1 norm: 0.96 | 
| horse  | Precision:0.96, Recall: 0.95, F1 norm: 0.95 | 
| ship   | Precision:0.95, Recall: 0.95, F1 norm: 0.95 | 
| truck  | Precision:0.94, Recall: 0.95, F1 norm: 0.95 | 


| class  | plane  | car    | bird   | cat    | deer   | dog    | frog   | horse  | ship   | truck  |
|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|--------|
| plane  | 0.93200| 0.00800| 0.01600| 0.00700| 0.00300| 0.00100| 0.00300| 0.00200| 0.02000| 0.00800|
| car    | 0.00400| 0.96400| 0.00000| 0.00000| 0.00000| 0.00000| 0.00100| 0.00100| 0.00600| 0.02400|
| bird   | 0.01800| 0.00000| 0.90300| 0.02200| 0.01900| 0.00900| 0.01500| 0.00800| 0.00400| 0.00200|
| cat    | 0.00600| 0.00200| 0.01400| 0.86900| 0.01700| 0.05900| 0.01400| 0.00700| 0.00600| 0.00600|
| deer   | 0.00300| 0.00000| 0.01000| 0.01500| 0.94700| 0.01000| 0.00300| 0.01000| 0.00000| 0.00200|
| dog    | 0.00400| 0.00100| 0.01100| 0.06200| 0.01900| 0.89200| 0.00100| 0.00800| 0.00000| 0.00200|
| frog   | 0.00500| 0.00100| 0.01200| 0.01400| 0.00800| 0.00300| 0.95100| 0.00200| 0.00200| 0.00200|
| horse  | 0.00400| 0.00000| 0.00600| 0.01200| 0.01100| 0.01300| 0.00000| 0.94800| 0.00200| 0.00400|
| ship   | 0.02200| 0.00400| 0.00600| 0.00300| 0.00000| 0.00200| 0.00100| 0.00100| 0.95500| 0.00600|
| truck  | 0.00700| 0.02800| 0.00100| 0.00200| 0.00000| 0.00000| 0.00000| 0.00000| 0.01200| 0.95000|

### Implementation of ResNet18 with exponentially decreasing learning rate

![](images/Graph2.png)


Best test accuracy: 0.92490, training accuracy: 0.99632 (cost:0.02136)

|  Class  |  Score                                      |
| ------- | ------------------------------------------- |
|  plane  | Precision:0.92, Recall: 0.93, F1 norm: 0.92 |
|  car    | Precision:0.95, Recall: 0.96, F1 norm: 0.96 |
|  bird   | Precision:0.92, Recall: 0.90, F1 norm: 0.91 | 
|  cat    | Precision:0.83, Recall: 0.85, F1 norm: 0.84 |
|  deer   | Precision:0.93, Recall: 0.94, F1 norm: 0.93 |
|  dog    | Precision:0.89, Recall: 0.89, F1 norm: 0.89 |
|  frog   | Precision:0.95, Recall: 0.95, F1 norm: 0.95 |
|  horse  | Precision:0.96, Recall: 0.93, F1 norm: 0.95 |
|  ship   | Precision:0.95, Recall: 0.95, F1 norm: 0.95 |
|  truck  | Precision:0.95, Recall: 0.95, F1 norm: 0.95 |

### Implementation of ResNet with 62 layers with exponentially decreasing learning rate

![](images/Graph3.png)

Best test accuracy: 0.91590, training accuracy: 0.99350 (cost:0.01992)

|  Class  |  Score                                      |
| ------- | ------------------------------------------- |
|  plane  | Precision:0.91, Recall: 0.93, F1 norm: 0.92 |
|  car    | Precision:0.95, Recall: 0.95, F1 norm: 0.95 |
|  bird   | Precision:0.89, Recall: 0.88, F1 norm: 0.89 | 
|  cat    | Precision:0.82, Recall: 0.82, F1 norm: 0.82 |
|  deer   | Precision:0.91, Recall: 0.93, F1 norm: 0.92 |
|  dog    | Precision:0.86, Recall: 0.86, F1 norm: 0.86 |
|  frog   | Precision:0.95, Recall: 0.93, F1 norm: 0.94 |
|  horse  | Precision:0.94, Recall: 0.93, F1 norm: 0.94 |
|  ship   | Precision:0.96, Recall: 0.94, F1 norm: 0.95 |
|  truck  | Precision:0.93, Recall: 0.94, F1 norm: 0.94 |


### Discussion
Note that a high precision, low recall for an object X means that the network is very cautious 
about this object. So for many objects which are X, the network might say "this is not X", but if it says
"it is X" then it is very likely to be correct. So its true guesses are very precise but can not 
recall many X's.

A high recall but low precision for an object X means that the network is very good at finding all the objects
which are X but it overshoots. So it identifies most of all the objects X as X but it also identifies many other
which are not X as X as well. So it is not very precise but recalls all the X.

In this case we see that cats and dogs have both lower recall and precision compared to others.
This likely means that the program confuses cats and dogs with each other. The confusion matrix for the first case confirms this
a cat is most likely to be confused with a dog (5 times more compared to other objects) followed by deers and birds and the same is true for the dog as well.

# Requirements

Pytorch 0.3 and internet connection to download the datasets.

# Author
Sina Tureli
Yet Another Implementation of Residual Networks


# Licensing

This repository is
[Apache-licensed](https://github.com/bamos/densenet.pytorch/blob/master/LICENSE).
