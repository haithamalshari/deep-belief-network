# deep-belief-network
A simple, clean, fast Python implementation of Deep Belief Networks based on binary Restricted Boltzmann Machines (RBM), built upon NumPy and TensorFlow libraries in order to take advantage of GPU computation:
> Hinton, Geoffrey E., Simon Osindero, and Yee-Whye Teh. "A fast learning algorithm for deep belief nets." Neural computation 18.7 (2006): 1527-1554.

> Fischer, Asja, and Christian Igel. "Training restricted Boltzmann machines: an introduction." Pattern Recognition 47.1 (2014): 25-39.

## Usage
This implementation follows [scikit-learn](http://scikit-learn.org) guidelines and in turn, can be used alongside it. Next you have a demo code for solving digits classification problem which can be found in **classification_demo.py** (check **regression_demo.py** for a regression problem and **unsupervised_demo.py** for an unsupervised feature learning problem).

Code can run either in GPU or CPU. To decide where the computations have to be performed is as easy as changing the parameter *mode*: if set to 'tf', it will do the computations on GPU (or CPU depending on you hardware) using TensorFlow; if set to 'np' it will compute on CPU using NumPy. **Note only pre-training step is GPU accelerated so far**. See the following snippet:

    import numpy as np

    np.random.seed(1337)  # for reproducibility
    from sklearn.datasets import load_digits
    from sklearn.cross_validation import train_test_split
    from sklearn.metrics.classification import accuracy_score

    from dbn import SupervisedDBNClassification


    # Loading dataset
    digits = load_digits()
    X, Y = digits.data, digits.target

    # Data scaling
    X = (X / 16).astype(np.float32)

    # Splitting data
    X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.2, random_state=0)

    # Training
    classifier = SupervisedDBNClassification(hidden_layers_structure=[256, 256],
                                             learning_rate_rbm=0.05,
                                             learning_rate=0.5,
                                             n_epochs_rbm=10,
                                             n_iter_backprop=100,
                                             l2_regularization=0.0,
                                             batch_size=32,
                                             activation_function='relu',
                                             dropout_p=0.2,
                                             mode='tf')  # set 'np' for using numpy on CPU
    classifier.fit(X_train, Y_train)

    # Test
    Y_pred = classifier.predict(X_test)
    print 'Done.\nAccuracy: %f' % accuracy_score(Y_test, Y_pred)

## Installation
1. [OPTIONAL] Install TensorFlow (if it isn't already) from https://www.tensorflow.org/ in case you want to use GPU.
2. Open a terminal and type the following line:

    pip install git+git://github.com/albertbup/deep-belief-network.git
