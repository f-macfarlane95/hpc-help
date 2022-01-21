Machine Learning
================

This page explains how to set up the tensorflow python module for accelerating machine learning using the
GPUs installed in the ``thanos`` and ``jaws`` nodes in the GPU partition. If you have other software
you prefer to use for machine learning you are free to try it out instead, but this page uses tensorflow
as an example.

Begin by logging in to the HPC onto the ``gruffalo`` server as normal.

Next request an interactive session on a gpu enabled compute node using srun::

  srun --partition=gpu --gpus=1 --mem=8G --cpus-per-task=4 --pty bash

Once connected to the compute node (assuming one is available), create a new conda environment
(you can pick any name for the environment) and activate it::

  conda create --name tensorflow-gpu-env
  conda activate tensorflow-gpu-env

Next install the tensorflow-gpu package::

  conda install tensorflow-gpu

We will now test the install to confirm that it can use the GPU to speed up training.
Begin by starting python::

  python

At the python prompt enter the following commands::

  import tensorflow as tf
  print( tf.constant( tf.__version__) )
 
This should print a long list of messages confirming that it has found a GPU to use.
Next we will train a model on some of the included test data that comes with the package::

  mnist = tf.keras.datasets.mnist
  (train_images, train_labels), (test_images, test_labels) = mnist.load_data()
  train_images = train_images.reshape(60000, 28, 28, 1)
  test_images = test_images.reshape(10000, 28, 28, 1)
  train_images, test_images = train_images/255, test_images/255
  model = tf.keras.Sequential([
    tf.keras.layers.Conv2D(32, (3,3), activation='relu', input_shape=(28,28,1)), tf.keras.layers.Conv2D(64, 
    (3,3), activation='relu'), tf.keras.layers.MaxPooling2D(2,2), tf.keras.layers.Dropout(0.25), 
    tf.keras.layers.Flatten(), tf.keras.layers.Dense(128, activation='relu'), tf.keras.layers.Dropout(0.5), 
    tf.keras.layers.Dense(10, activation='softmax')
  ])
  model.compile(optimizer='adam', loss='sparse_categorical_crossentropy', metrics=['accuracy'])

We are now ready to run the training, which should take about 30 seconds to run using GPU acceleration::

  model.fit(train_images, train_labels, batch_size=128, epochs=15, verbose=1,
            validation_data=(test_images, test_labels))

In production use you will obviously not have to create the conda environment or install tensorflow-gpu as you've now set those up in the above example. To run your own code you only need to use the srun commands to access the gpu node and then conda activate tensorflow-gpu-env.

Tutorial Series
---------------

In this section tutorials for installing various common Machine Learning networks on the cluster will be given. 
Many existing tutorials exist, however they are often incomplete or outdated and do not consider the specific versions of packages required as they were written at the time when each package was current. In addition, each tutorial will cover running a demo of each network on the cluster and interacting via ``jupyter-server`` within a local browser window.


Mask RCNN
~~~~~~~~~

:doc:`ml-tut-maskrcnn` [`paper <https://arxiv.org/abs/1703.06870>`_, `code <https://github.com/matterport/Mask_RCNN>`_] is a model used for instance segmentation, i.e., counting and segmenting the number of instances of various object classes within an image.



