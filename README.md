# image-node
c++ serving of machine-learning graphs using nodejs

This started out as a toy project to serve one of the tensorflow c++ examples through nodejs.
It's still toylike but it's more promising than I expected.
The idea is to allow a simple microservice approach - import graphs into a c++ tensorflow environment, build these as sharedlibs and connect to them using nodejs addons. 

At present it provides access to 4 ml applications:
- The tensorflow inception model: updated to v4 (it is also possible to use other versions)
- The tensorflow bundled multibox model
- A tensorflowification of a Joint Face Detection and Alignment using Multi-task Cascaded Convolutional Networks implementation
- A tensorflowification of a Single Shot Detector

The 4 give a good spread of input-response situations.
The smallest model - face detection and alignment needs more work as it is not a 'pure' network, cascading from one network to another:
I've provided a python notebook that leads through the tensorflow graph that we freeze and export for use in c++.

Here's a basic output: 

![alt tag](/resources/canoe_output.jpg) 

So we have 
- face position and alignment
- person detection
- classification
- object localization

Nice when it gets it right!

To build the c++ code you'll need tensorflow and bazel - This builds fine on my osx and linux installs and also on docker images
running on my machines, I've not had any problems so far except when my tensorflow and bazel versions have got out of sync.
Easily solved.

There's a detailed run through of getting some of the parts working [here](http://www.xythings.com/thoughts/articles/24022017.html) 

A simple way to get the python notebook running is to use one of tensorflow docker images with the py directory contents.

The inception model can be downloaded from [here](http://download.tensorflow.org/models/inception_v4_2016_09_09.tar.gz) and converted to
the pb format using the py notebook in [py](https://github.com/illmade/image-node/tree/master/py/inception_v4.ipynb)

And the multibox model from [here](https://storage.googleapis.com/download.tensorflow.org/models/mobile_multibox_v1a.zip)

Place both of them in the cc/data directory.
Edit the path to your data in addons/classify.cc, where you can also set the log level of the output

ln106: multiClassify.reset(new MultiClassify("..path to directory containing data/ ", "INFO"));

You should be able to create the pb models using the python notebooks but they are also available [here](https://drive.google.com/drive/folders/0B9-SnYo1_fxZcVpQa2RuUDBLS0E)

Change INFO to DEBUG1 for a debug output.

I've got this running on osx and in docker images - the next step is to try out images on amazon and google and do some performance analysis.

This is pretty much *my first c++ app* so I'm sure it's far from perfect: though it's stable enough for an alpha phase of development.
