# Tensorflow Environment setup

- Install Anaconda from https://www.anaconda.com/download/#macos . Anaconda is a popular package, dependancy and environment manager for python. During installation `Add Anaconda to my path variable`.
- You can install preconfigured .yml files using the command `conda env create -f file_name.yml`
-` You activate environment files using `source activate name_of_environment_specified_in_yml`. You can use `source deactivate` to deactivate the environment.
- We then type `jupyter notebook` to start the server.

## Jupyter notebook
Jupyter notebook App is a client side app that allows editing and running Python in the browser (as well as many other things). You can create notebook notes that are a combination of computer code and markdown. You need to run cells that include variable definitions before you  

## Machine Learning

Unlike typical computer programs, machine learning techniques will iteratively learn from data.
Machine learning algorithms can find insights in data even if they aren't specifically instructed what to look for in the data. You give the algorithm rules to follow rather than what to look for.
Three major types of machine learning are:
- Supervised learning
- Unsupervised learning
- Reinforcement Learning

### Supervised Learning
Supervised learning uses *labelled* data to predict a label given some features. If the label is continuous it is called a regression problem, if it is categorical it is a classification problem.
Classification example: Your features could be height and weight and your label could be gender. Given a persons height and weight predict their gender. We train our machine learning algorithm on data and then if we give a height and weight, the algorithm could predict if it belongs to male or a female.
Regression example: In regression the label rather than being categorical is continuous e.g features are square footage and rooms and the label is house price. Given a house's size and number of rooms predict the house price.
Supervised learning has the model train on historical data that is already labelled (e.g previous house sales). Once the model is trained, then it can be used on new data, where only the features are known, to attempt prediction.
### Unsupervised learning
What if you don't have any historical labels for your data,  you only have features. Since you have no right answers to fit on, you need to look for patterns in the data and find a structure.
*Clustering*
What if your features are heights and weights for breeds of dogs, but you don't have any labels for the dog breeds.You must cluster the data into similar groups it is up to the data scientist to interpret the clusters.
Clustering won't be able to tell you what the group labels should be. Only that the points in each cluster are similar to each other based on the features.

### Reinforcement Learning
What about machine learning tasks like have a machine learn how to play a video game, drive a car etc...?
Reinforcement learning works through trial and error, which actions yield the greatest rewards.
Components:
- Agent - Learning/decision maker
- Environment - What the agent interacts with
- Actions - What the agent can do

The `agent` chooses some `actions` that maximise some specified reward metric over a given amount of time. Learning the best policy with the `environment` and responding with the best `actions`.

## Basic process for a supervised machine learning problem
- Acquire data from a datasource.
- Clean and organise the data. You need to process the data by normalising the data, reducing the pixel count etc. This is the most time consuming part.
- Train test split. You normally train with 70% and test with 30%.
- You train your model solely on the training set.
- You then evaluate your model using your test set.
- You can then adjust model parameters and evaluate the model again to try and get a better fit on the test set.
- Once your satisfied with your model you can deploy it.

For unsupervised learning, you use all the data as training data and then evaluate based of an unsupervised learning metric.
Some times you use a holdout set. You split your data into 3 sets and use the holdout data to evaluate the model after the model has been tested against the test data. This gives us some sort of final idea of how the model will perform when it's deployed. It allows us test the model against data it truly hasn't seen or been adjusted for. You aren't supposed to adjust parameters after you use the holdout data set.

### Model Evaluation
Supervised learning categorical - we can evaluate by looking at things like accuracy, recall and precision. Accuracy is the number of correctly evaluated samples measured against the total number of samples.
Which metric is most important depends on the task you're undertaking.
Supervised learning regression - Mean absolute error, mean square error, root mean square error. All are essentially measurements of, on average how far off are you from the correct continuous value.
Unsupervised learning - Much harder to evaluate and depends on the overall goal of the task. It never had the correct labels to begin with. You can look at cluster homogenity or the Rand index.
Reinforcement learning - Evaluation is usually more obvious as the "evaluation" is built into the actual training of the model. How well the model performs the task it's assigned.

## Crash course in Python libraries

### Numpy
We can use numpy to transform variables in Python into arrays and to create arrays of numbers very quickly.
```
import numpy as np
my_list = [1,2,3] // Not sure why python has lists not arrays?
np.array(my_list) // converts the list into an array
np.arange(0,10) // this creates an array for 0 to (but not including) 10.
np.arange(0,10, 2) // uses every other element
```
`np.zeros` creates an n dimensional array of zeros based on the parameters passed in.
`np.linspace(start, end, number_in_array)` linspace creates a linear array based on variables passed in.
```
np.zeros((3,5))
array([[ 0.,  0.,  0.,  0.,  0.],
       [ 0.,  0.,  0.,  0.,  0.],
       [ 0.,  0.,  0.,  0.,  0.]])

np.linspace(0,11,11)
array([  0. ,   1.1,   2.2,   3.3,   4.4,   5.5,   6.6,   7.7,   8.8,
         9.9,  11. ])
```

`np.random.randit(start, end)`
You can set a random seed that will make sure you return the same random numbers each time.
```
np.random.seed(101)
np.random.randint(0,100,10)
```
there are many array helpers
```
arr.max
arr.min
arr.mean
arr.argmax // returns the index of the maximum value
```

### Pandas
Pandas can read a csv files and perform operations on them. It is based on numpy and has similar functions

### Data Visualisation

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

%matplotlib inline // magic command for jupyter notebook
plt.show() // Does the same thing as above but in other environments
```

### SciKit-learn for data pre-processing
We use the MinMaxScaler from Scio-kit learn in order to normalise our data.
```
from sklearn.preprocessing import MinMaxScaler

scaler_model = MinMaxScaler()
scaler_model.fit(data)
scaler_model.transform(data)
scaler_model.fit_transform(data)
```
You normally fit to your training data and then you transform your test data and your training data
```
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=101)

```

## Neural networks
### Introduction to Perceptrons
The single component of a neural network is a neuron. Artificial Neural Networks have a basis in biology. We can attempt to mimic biological neurons with an artificial neuron known as a perceptron.
- Biological neurons have dendrites that pass electrical singles to the body. It then passes out a single output
- A perceptron can have multiple inputs and a single output.
Inputs will be the values of features. These inputs are then multiplied by some sort of weight. The weights are typically initialised randomly. the inputs are multiplied by the weights. These results are then passed into an activation function. A simple example is if the sum of the inputs is positive output 1, if negative output 0.
Theoretically if both the inputs were 0 the neural network wouldn't work. We can add in a bias term that keeps the system working.
### Multiple perceptrons network
A perceptron network has an input layer, a hidden layer and an output layer.
- Input layers are real values from the data
- Hidden layers are layers between the input and the output. 3 or more layers is a deep networks
- Output layer is the final estimate of the output.
As you go forwards through more layers the level of abstraction increases.

### The Activation function
Changing the activation function used can be beneficial depending on the the task. Common functions are:
- the Sigmoid function (output from 0 - 1)
- the Hyperbolic tangeant function (output from -1 to 1). tanh
- ReLu - the rectified linear unit (you pass in your z value, and based on you z value return the maximum it can be)
ReLu and tanh tend to have the best performance.
Deep learning libraries have these built in for us, so we don't need to worry about implementing them manually.
### Cost functions
We can evaluate the performance of a neuron using a cost function. It measures how far off we are from the expected value.
We'll use the follwing variables:
- y is the true value
- a is the neuron's prediction
In terms of weight and bias:
- w*x + b = z (weight x input + bias = z)
- Pass z into the activation function.
Quadratic cost function:
- C = sum of the true value - the prediciton squared, divided by
Large error are more prominent due to the squaring.
Unfortunately this calculation can cause a slowdown in our learning speed.

Cross Entropy:
- C = bloody complicated
This cost function allows for faster learning. The larger the difference, the faster the neuron can learn.

### Gradient descent and Backpropogation (or actually learning)
Gradient descent is an optimization algorithm for finding the minimum of a function.
To find a local minimum we take steps proportional to the negative of the gradient.
The way gradient Decent works is by taking the gradient and seeing which way it decends until it has no gradient and we get the minimum cost. Visually we can see what parameter to choose to minimise our cost.
Finding this minimum is simple for 1 dimension, but our cases will have many more parameters, meaning we'll need to use the built in linear algebra that our Deep Learning library provides.
Using Gradient Descent we can figure out the best parameters for minimising our cost, for example, the best values for the weights of the neuron inputs.

Backpropogation is how we quickly adjust the optimal parameters or weights across our network.
Backpropogation is used to calculate the error contribution of each neuron after a batch of data is processed.
It relies heavily on the chain rule to go back through the network and calculate these errors.
Backpropogation works by calculating the error at the output and then distributes back through the network layers.
It requires a known desired output for each input value (supervised learning).

## Manual Neural Networks
### Operation Class
The operation class will have
- Input Nodes
- Output Nodes
- Global Default Graph Variable
- Compute which is overwritten by extended classes

A Graph is essentially a global variable. Tensorflow runs off graphs. A graph is a list of nodes nodes tends to be constants and operations.
### Placeholders, Variables and Graphs
A Placeholder is an empty node that needs a value to be provided to compute an output. It is a placeholder for data that we provide
Variables are the changeable parameter of the Graph (e.g weights)
Graph is a global variable connecting variables and placeholders to operations.
### Sessions
Now that the Graph has all the nodes, we need to execute all the operations within a session.
We'll use a PostOrder Tree Traversal to make sure we can execute the nodes in the correct order.

## Tensorflow
### Basic syntax
```
import tensorflow as tf

hello = tf.constant("Hello ")
// this creates a tensor. A tensor is a fancy name for an n dimensional array
world = tf.constant("world")

// Creating a Session
with tf.session() as sess():
  result = sess.run(hello + world)
```
### graphs
- Graphs are sets of connected Nodes (vertices)
- The connections are referred to as edges
- In Tensorflow each node is an operation with possible inputs that can supply some output
There are two main types of tensor objects in a Graph
- variables
- placeholders
### Variables:
- During the optimization process tunes the parameters of the model.
- Variables can hold the values of weights and biases throughout the Session
- Variables need to be initialised
### Placeholders:
- Placeholders are initially empty and are used to feed in the actual training examples.
- However they do need a declared expected data type (tf.float32) with an original shape argument.

### Estimators
Tensorflow's estimator api has several model types to choose from.
- `tf.estimator.LinearClassifier` constructs a linear classification model
- `tf.estimator.LinearRegressor` constructs a linear regression model.
- `tf.estimator.DNNClassifier` constructs a neural network classification model
- `tf.estimator.DNNRegressor` constructs a neural network regression model
- `tf.estimator.DNNLinearCombinedClassifier` constructs a neural network and linear combined classification model.
- `tf.estimator.DNNLinearCombinedRegressor ` constructs a neural network and regression combined classification model.

In general, to use the estimator api we do the following:
- Define a list of feature columns
- Create the Estimator Model
- Create a Data Input Function
- Call train, evaluate and predict methods on the estimator object.

## New Theory Topics

### Initialisation of weights options:
zeros:
- No randomness
- Not a great choice
Random Distribution Near zero:
- Not optimal
- Activation Functions Distortion
Xavier (Glorot) initialization:
- Uniform / Normal
- Draw weights from a distribution with zero mean and a specific variance

### Gradient Descent
Learning rate defines the step size during gradient descent. If you choose too low a learning rate you may descend too slowly never reach the convergence within a reasonable time frame. If you choose too high you may end up overshooting your minimum and never reach the convergence rate either.
Batch size allow us to use the stochastic form of gradient descent. If we fed all our data into the neural network at once it'd be computationally too expensive.
- Smaller are less representative of data
- Larger give a longer training time
Second-Order Behaviour of the gradient descent allows us to adjust our learning rate based off the rate of descent. It would be good if at the beginning when the error was large, the rate of descent was also large and it would adjust as the error gets smaller:
- AdaGrad
- RMSProp
- Adam
Unstable/Vanishing Gradients.
As you increase the number of layers in a network, the layers towards the input will be affected less by the error calculation occurring at the output as you go backwards through the network.
- Initialisation and Normalization will help us mitigate these issues.

### Overfitting vs underfitting a model_selection

A large error on both your training set and your test set you are under-fitting your model.
A tiny error on your training data but a huge error on your test data is over-fitting.
You need to strike a balance.
With potentially hundreds of parameters in a deep learning neural network, the possibility of overfitting is very high!
There are a few ways to help mitigate this issue.
L1/L2 Regularization
- adds a penalty for larger weights in the model.
- not unique to neural Networks
Dropout
- Unique to neural Networks
- Remove neurons during training randomly
- Network doesn't over rely on any particular neuron
Expanding data
- Artificially expand data by adding noise, tilting images, adding low white noise to sound data etc.

## The MNIST data set

The MNIST data set contains handwritten single digits from 0 - 9.
A single digit image can be represented as an array on a 2d plane.
Specifically a 2d array of 28 pixels by 28 pixels. The values represent a grayscale image.
We can then flatten this array to a 1-D vector of 784 numbers. Either (781,1) or (1, 781) is fine, as long as the dimensions are consistent.
Flattening out the image ends up removing some of the 2-D information, such as the relationship of a pixel to it's neighbouring pixels.
We can think of the entire group of the 55,000 images as a tensor (an n-dimensional array).
For the labels we'll be using One-Hot Encoding. This means that instead of having labels such as "One", "Two", etc... we'll have a single array for each image.
- The label is represented based off the index position in the label array.
- The corresponding label will be a 1 at the index location and zero everywhere else.
- For example, 4 would have this label array: [0,0,0,0,1,0,0,0,0]
As a result, the labels for the training data ends up being a large 2d array.

## Softmax regression

The Softmax regression approach is a common layer approach. It's similar to previous methods used.
- A Softmax regression returns a list of values between 0 and 1 that sum up to 1.
- This can be treated as a list of probablities so if we have 10 potential labels we can then get a list of 10 probabilites.
- It will choose which label has the highest likelihood of being correct. One number will have a very high percentage (e.g 95) then there may be a 4 and then a few tiny numbers, but they always add up to 100.

We'll use softmax as our activation function. We sue it to define a new type of output layer for our model.

```
# EVALUATE THE MODEL
   correct_prediciton = tf.equal(tf.argmax(y,1),tf.argmax(y_true,1))

   # PREDICTED [3,4] TRUE [3,9]
   # [TRUE, FALSE]
   # WE THEN CAST THIS INTO A FLOAT [1.0, 0.0]
   # AND TAKE AVERAGE 0.5 WHICH IS 50% ACCURACY

   # return the index position of the label with the highest
   acc = tf.reduce_mean(tf.cast(correct_prediciton, tf.float32)) # converts true and falses to ones and zeros



```

## Convolutional Neural Network Theory
- CNNs also have their origins in biological research.
- Hubel and Weisel studied the structure of the visual cortex in mammals.
- Neurons in the visual cortex had a small local receptive field. They are only looking at a smaller sub section of the whole image, but they can overlap and create a larger image and visual field. They realise they are only active when a certain shape is detected.
- This idea inspired the architecture that would become CNN


### Tensors
Tensors are just N-Dimensional arrays that we build up to:
- Scalar - 3
- Vector - [3,4,5]
- Matrix - [[3,4], [5,6], [7,8]]
- Tensor - [[[1,2]. [3,4]],
            [[5,6], [7,8]]]
Tensors make it convenient to feed in sets of images into our model - (I,H,W,C)
 - Images
 - Height of image in pixels
 - Width of image in pixels
 - Color of channels: 1-Grayscale, 3-RGB

### DCNNs vs CNNs
 We can create densely connected neural networks with the tensorflow api.
 It is when each tensor in one layer is connected to the tensor in the next layer.
 For a convolutional layer we take a different approach. Each unit is connected to a small number of nearby units in the next layer.
 Most images are at least 256 x 256 pixels or greater (>56k total)! A densely connected neural network would lead to way too many parameters and would make it unscalable to use new images.
Convolutions also have a major advantage for image processing, where pixels nearby to each other are much more correlated to each other for image detection.
- Each CNN layer looks at an increasingly larger part of the image.
- Having units only connected to nearby units also aids in invariance.
- CNN also helps with regularization, limiting the search of weights to the size of the convolution.

The input layer is the image itself. Convolutional layers are only connected to pixels in its local field. As you get towards the edge of an image there may not be any image left, but we can fix this by adding a padding of zeros around the outside.

With 1 dimensional convolutions, we can treat the weights as a filter.
You can use different filters to create different outputs for each image.

## Pooling layers
Pooling layers will subsample the input image, which reduces the memory use and computer load as well as reducing the number of parameters.
- Each image is a grid of pixels and each pixel has a value representing it's colour.
- With pooling you create a 2 by 2 pool of pixels and evaluate their maximum value
- Only the maximum value makes it to the next layer
- Then we move over by a stride  and takes the next max value.
- This pooling layer will end up removing a lot of information, even as a small pooling "kernel" of 2 by 2 with a stride of 2 will remove 75% of the input data.
