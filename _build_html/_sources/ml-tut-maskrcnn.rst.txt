:orphan:

Mask RCNN
=========

Building an Anaconda environment
________________________________

First log into ``gruffalo`` in the standard way::

	ssh <username>@gruffalo.cropdiversity.ac.uk
	
Once connected create and activate a new Anaconda environment for the Mask R-CNN installation, specifically in ``python 3.6.12`` to avoid version conflicts::
	
	conda create -n maskrcnn_example python=3.6.12
	
	conda activate maskrcnn_example

.. note::
	Python 3.6.12 is required for Tensorflow version 1.15. Alternatively, tutorials exist to update the model used to be compatible with more recent versions of both python and TensorFlow.

The implementation of Mask R-CNN used can be found on `GitHub <https://github.com/matterport/Mask_RCNN>`_. Clone this repository into the ``include`` folder of the conda environment::
	
	cd path/to/conda/envs/include
	
	git clone https://github.com/matterport/Mask_RCNN.git

The required packages are listed in ``requirements.txt`` in the new Mask_RCNN folder and could be installed from this, however, they automatically install the latest versions of each of the required packages which no longer work with the default model. Instead install the following packages using ``conda install``:

* numpy
* scipy 
* Pillow
* cython
* matplotlib
* scikit-image
* tensorflow=1.15
* tensorflow-gpu=1.15
* keras=2.2.5
* h5py
* imgaug
* IPython

where ``*=x.xx`` specifies which version of the package to install. In ``conda`` this is marked by a single "=" whereas in ``pip`` it is denoted as "==".
Alternatively, update the list in ``requirements.txt`` with the above and use ``conda install -f requirements.txt`` to install everything in a single command.

.. note::
	Installing from a single file will mean conda searches for a valid combination of all packages, unless versions are specified, which can take an excessive amount of time. It is reccomended to install each one-by-one allowing for individual conflicts to be resolved ad hoc.


Additionally, install "opencv-python" using ``pip install`` as well as pycocotools using the following::

	pip install git+https://github.com/philferriere/cocoapi.git#subdirectory=PythonAPI

Finally, if not already there, navigate to the install location and run::

	python setup.py install
	
If you encounter an error, make sure you are in the correct folder and have installed each of the required packages.	


Configuring port forwarding for jupyter notebooks
_________________________________________________

Verifying the installation and performing object detection
__________________________________________________________


Training a custom dataset
_________________________


Sourcing and Labelling Images
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Formatting the custom Dataset for input into Mask RCNN
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Transfer Learning vs Full Weight Training
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Tuning a custom model
~~~~~~~~~~~~~~~~~~~~~


Monitoring training progress 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Training, especially from scratch on a deep network, takes hours and sometimes even days. Therefore, it is important to monitor and fine tune the training process and ensure the model is behaving. 

Many tensorflow models include what are known as "callback" variables in order to monitor training. In the implementation of Mask RCNN we are using, custom callbacks can be added in the form of variables or functions on line 2346 of ``model.py``. 

Method 1: Viewing via the HPC Terminal
""""""""""""""""""""""""""""""""""""""
Most, if not all, models will have some basic training and validation monitoring built into the source. This is often in the form of printing updates directly to the command window such as the current training step and epoch as well as loss values.

In order to ensure the status remains visible, even in the event of a disconnection from gruffalo, the ``screen`` command can be used to create a persistent terminal that can be accessed.

Method 2: Viewing via a Tensorboard Server
""""""""""""""""""""""""""""""""""""""""""
Alternatively, callbacks can be viewed in Tensorboard, which watches the log files being generated during the training process and graphically displays the progress of these training values after each epoch. 

.. note::
	Unlike the terminal based feedback from the previous method which gave stepwise updates, these values are only updated after each successive epoch.


Run Tensorboard using the following, ensuring to provide arguments for both the watched port as well as a path to the logs directory inside your mask RCNN installation being watched::

	ssh -L 12345:localhost:12345 -J <username>@gruffalo.cropdiversity.ac.uk <username>@twiki

	tensorboard --port 12345 --logdir ../path/to/logs/directory

.. note::
	If the "logs" folder contains multiple subfolders, each of these will be superimposed onto the graphs shown in tensorboard which is useful for comparing the performance of multiple models or versions.


Inference (Testing)
___________________


