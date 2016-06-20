# Object Detecion: from Viola-Jones to 
Convolutional-Neural-Netoworks-based object detector

### Problem formulation.
The task of object detection consists of [(1)][obj_detect_wiki]:
 - Finding the locations of all the instances of an object in an image.
 - Determining the sizes of all found instances.
 - Having a certain amount of invariance to view-point changes, 
illumination changes and occlusions.
 - Depending on the application, being able to do it in real-time or 
speed that makes batch processing feasible.

One of the examples of an object to be detected is a human face. The 
respective task is called Face Detection. In this case, an input for the 
algorithm will be an image that might or might not contain faces. The 
algorithm should find positions of rectangles that contain face along 
with width and hight of an rectangle. By doing so, the algorithm detects 
faces and their sizes. At the same time, algorithm should have 
invariance to:

 - Intra-class variations: faces of babies and faces of people with the 
beard should also be detected; 
 - Pose and expression variations: slightly tilted faces and faces with 
different emotional expression should be detected.
 - Occlusions: people wearing glasses or partially covering their face 
with a hand should be detected.

Usually, the task of Face Detection should be performed in real-time 
[(2)][viola_jones]. For example, some cameras has a built-in Face 
Detection algorithm that enables cameras to adjust focus and exposure to 
provide the best result [(3)][face_detection_camera_case], which should 
be done in real time.

### Talk overview.

In the talk we plan to cover these topics:
 - Algorithmic stages of the first real-time Object Detection framework 
by Viola and Jones [(2)][viola_jones], that was originally developed for 
the task of Face Detection [(2)][viola_jones].
 - Available implementation of the algorithm in OpenCV 
[(4)][opencv_viola] and its problems. Overall complexity of installation 
of OpenCV for usual users.
 - Our implementation of the algorithm [(5)][obj_detect_module] that can 
be fully integrated into scikit-image image processing library 
[(4)][skimage].
 - Description of one of the modern approaches to object detection using 
Convolutional Neural Networks [(6)][cascade_cnn] that combines accuracy 
of Convolutional Neural Networks [(7)][cnn_success] and speed by using 
attentional cascade approach from the original Viola-Jones paper 
[(2)][viola_jones].

### Viola-Jones object detector.

In the algorithmic stage section, the main blocks of the Viola-Jones 
algorithms are described:
- Haar features [(2)][viola_jones] and MultiBlock-Local-Binary Patterns 
(MB-LBP) [(8)][mb-lbp] features that were shown to give better results 
[(8)][mb-lbp].
- Integral images -- a special representation of the image, that enables 
all the features like Haar and MultiBlock-Local-Binary Patterns to be 
computed in a constant time on any scale [(2)][viola_jones].
- Adaboost feature selection technique -- an approach that selects a 
certain amount of features(Haar or MB-LBP) to create a best performing 
weak classifier [(2)][viola_jones].
- Attentional cascade -- an algorithmic approach that helps to combine 
weak classifiers into a strong one, while at the same time speeding up 
the evaluation process. It quickly rejects the easy example in the first 
stages of cascade, and carefully evaluates a small number of challenging 
candidates in the last stages [(2)][viola_jones] [(9)][adaboost_wiki].

### OpenCV implementation and related problems.

In this section we colver those topics:
- OpenCV implementation. TBB library speed up, that allows us to make 
use of multiple threads while training the classifier. GPU 
implementation.
- Complexity of installation of OpenCV. Although OpenCV provides highly 
efficient implementations, first time users experience problems. 
Easy-install and easy-use solution is missing.
- Problem with patent, that is not well-documented.


### Our implementation.

In the section describing our implementation we cover [(10)][our_blog]:
- The choice of MB-LBP features over Haar to speed up the computation.
- Cython implementation of critical sections.
- OpenMP library usage in Cython to employ multiple threads.
- Smart code compilation that determines if OpenMP is supported.
- Implementation that avoids the patent, analogous to OpenCV.
- Training dataset creation. Negative and positive training examples 
creation.
- Implementation that makes it possible to use xml files with weights 
from OpenCV.
- Possible future usage of Cuda to improve performance.


### Convolutional-Neural-Netoworks-based approach

- Recent success of Convolutional Neural Networks (CNNs) in multiple 
areas over approaches based on hand-crafted features like Haar features 
and MB-LBP [(7)][cnn_success].
- Recent hybrid approach that combines accuracy of CNNs and speed that 
is achieved by using Attentional Cascade [(6)][cascade_cnn].
- Description of the overall algorithm of the paper.

### Conclusion and discussion

In our talk, we introduced the first real-time object detection 
algorithm, explained why it achieves high accuracy while being 
real-time. We briefly described available implementation by OpenCV 
library, showed that while having outstanding performance it lacks a 
user-friendly and easy-to-use installation and API. After that we 
described our Python/Cython implementation that uses only scikit-image 
image procesing library along with OpenMP, making the installation 
process and usage easy. We concluded with the overview of the recent 
successful CNNs based approach that combines best of two worlds: CNN 
accuracy and real-time performance that was made possible by the usage 
of Attentional Cascade.   

   [obj_detect_wiki]: <https://en.wikipedia.org/wiki/Face_detection>
   [viola_jones]: 
<http://www.vision.caltech.edu/html-files/EE148-2005-Spring/pprs/viola04ijcv.pdf>
   [face_detection_camera_case]: 
<http://www.photoreview.com.au/tips/shooting/insider-how-useful-is-in-camera-face-detection>
   [opencv_viola]: 
http://docs.opencv.org/2.4/modules/objdetect/doc/cascade_classification.html
   [skimage]: http://scikit-image.org/
   [obj_detect_module]: 
https://github.com/scikit-image/scikit-image/pull/1570
   [cascade_cnn]: 
http://users.eecs.northwestern.edu/~xsh835/assets/cvpr2015_cascnn.pdf
   [cnn_success]: 
http://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf
   [mb-lbp]: 
http://www.cbsr.ia.ac.cn/users/scliao/papers/Liao-ICB07-MBLBP.pdf
   [adaboost_wiki]: https://en.wikipedia.org/wiki/AdaBoost#Overview
   [our_blog]: http://warmspringwinds.github.io/
