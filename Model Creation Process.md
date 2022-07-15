# CS50AI
## Week 5 - Traffic
### Model Experimentation Process

To keep the experimentation process less hectic the following parameters were kept unchanged.
`optimizer="adam",`
`loss="categorical_crossentropy",`
`metrics=["accuracy"]`
And output layer: `tf.keras.layers.Dense(NUM_CATEGORIES,activation = "softmax")`

#### Hidden layers
I started with one Dense layer and changed its size for every iteration.
	Sizes tried: 16, 32, 64, 128, 256, 512, 1024, 2048, 4096, 8192, 16384.
		- Accuracy was low(around 5%) till size = 1024.
		- Accuracy gradually increased from 5% to 40% as size increased from 2048 to 16384.

Then I repeated the same experiment with 2 Dense layers.
	- Again the accuracy was around 5% to 6%

I repeated the same experiment with 3 Dense layers.
	- Accuracy increased from 5% to 80% when size increased from 16 to 4096.
	- Size 128 and 512 were performed well among the batch. (Training & Testing Accuracy: **79**%-**80%**) 

I repeated the same experiment with 4 Dense layers.
	- The accuracies were same as the experiment with 3 dense layers.
	- But training and testing was significantly slower than the previous experiment.

So I decided to move to the next phase with 2 and 3 hidden layers of size 128 and 512.

#### Convolution Layers
Initially I used 1 Conv2D layer with kernel size 3x3 are experimented with different number of channels(16,32,64,128,256,512).
    
- Number of channels = 32,64,128 were performing better among the group, with training and testing accuracies of ~**97%** and ~**90%** respectively across all 4 runs -> ( 3 Dense layer of size 128, 3 Dense layer of size 512, 2 Dense layer of size 128, 2 Dense layer of size 512).

Then I experimented with different number of convolution layers(2 to 4) and different number of channels( 16, 32, 64, 128).

- All were performing almost the same with training and testing accuracy **97-98**% and **93-94**% respectively.
- Hidden layer size 512 was at the top of the group in terms of performance, so I stuck to 2 Dense layers of size 512 for future runs.
- 3 convolution layers were performing on par with 4 layers but were significantly faster, so I stuck to 3 layers for future runs.
##### Kernel Size
I experimented with 3x3, 5x5, 7x7, 9x9 for all the previous scenarios and only 3x3 performed well and as the kernel size increased the accuracy decreased.

So I stuck to 3x3 kernel size for future runs.

##### Different combinations of channels
I tried with different number of channels in each layer and out of all the combinations, one performed well among others.

The performance was better when the number of channels where gradually increased from 16 to 128. Accuracies were around **98.6%** and **97.5%** for training and testing respectively.

https://github.com/peter0471/Road-Sign-Recognition-/blob/main/Screenshot_5.png
#### Pooling Layers
I am using MaxPooling2D for all runs.
I experimented with maxpool and the performance dropped if I place pooling layers in-between convolution layers, so I placed it after all the convolution layers.

Placing it there bumped the performance a little bit to be consistently above **98.7%**(training) and **97.5%**(testing). It greatly reduced the parameters needed to be trained and made the whole process a tiny bit fasted and reduced the memory needs drastically.

I experimented with different pooling layer sizes (2x2, 3x3, 4x4, 5x5, 6x6, 7x7). The performance was not greatly affected in all these runs. Every size except 2x2 were a tad bit less than 2x2. So I decided to keep it 2x2.

#### Dropout Layer
I placed the dropout on the first hidden dense layer, since it had the most number of trainable parameters and placing the dropout on that layer would be the most effective.

I experimented with different dropout values(0.1, 0.2, 0.3, 0.4)
- 0.4, 0.3, 0.2, reduced the training accuracy to around 98% and did a good job in matching the testing accuracy with the training.
- 0.1 did not affect the training accuracy and also did a great job in increasing the testing accuracy to **98.5%**.
So I fixed 0.1 as the dropout value.

### The Final Model
https://github.com/peter0471/Road-Sign-Recognition-/blob/main/Screenshot_6.png

This is the best performing model from my experiment. It has training and testing accuracy of **99%** and **98.5%** respectively. The model appears to fit the training data well without overfitting, and generalises well to the testing data.
