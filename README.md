# SFND 2D Feature Tracking

<img src="media/2d_feature_tracking.gif" width="820" height="248" />

The idea of the camera course is to build a collision detection system - that's the overall goal for the Final Project. As a preparation for this, you will now build the feature tracking part and test various detector / descriptor combinations to see which ones perform best. This mid-term project consists of four parts:

* First, you will focus on loading images, setting up data structures and putting everything into a ring buffer to optimize memory load. 
* Then, you will integrate several keypoint detectors such as HARRIS, FAST, BRISK and SIFT and compare them with regard to number of keypoints and speed. 
* In the next part, you will then focus on descriptor extraction and matching using brute force and also the FLANN approach we discussed in the previous lesson. 
* In the last part, once the code framework is complete, you will test the various algorithms in different combinations and compare them with regard to some performance measures. 

See the classroom instruction and code comments for more details on each of these parts. Once you are finished with this project, the keypoint matching part will be set up and you can proceed to the next lesson, where the focus is on integrating Lidar points and on object detection using deep-learning. 

[//]: # (Image References)

[image1]: ./media/ring_buffer.png "Ring Buffer"
[image2]: ./media/deque_databuffer.png "Data buffer"
[image3]: ./media/AKAZE_detector.png "Akaze"
[image4]: ./media/Brisk_detector.png "Brisk"
[image5]: ./media/Fast_detector.png "Fast"
[image6]: ./media/Harris_corner_detector.png "Harris"
[image7]: ./media/ORB_Detector.png "ORB"
[image8]: ./media/Shi_Tomasi_detector.png "ShiTomasi"
[image9]: ./media/SIFT_detector.png "SIFT"
[image10]: ./media/ORB_AND_ORB.png "ORB and ORB"
[image11]: ./media/keypointRemoval.png "Focus on car"
[image12]: ./media/FAST_and_ORB.png "FAST and ORB"
[image13]: ./media/BRISK_and_SIFT.png "BRISK and SIFT"
[image14]: ./media/ORB_and_ORB.png "ORB and ORB"
[image15]: ./media/AKAZE_and_AKAZE.png "AKAZE and AKAZE"

## Dependencies for Running Locally
1. cmake >= 2.8
 * All OSes: [click here for installation instructions](https://cmake.org/install/)

2. make >= 4.1 (Linux, Mac), 3.81 (Windows)
 * Linux: make is installed by default on most Linux distros
 * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
 * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)

3. OpenCV >= 4.1
 * All OSes: refer to the [official instructions](https://docs.opencv.org/master/df/d65/tutorial_table_of_content_introduction.html)
 * This must be compiled from source using the `-D OPENCV_ENABLE_NONFREE=ON` cmake flag for testing the SIFT and SURF detectors. If using [homebrew](https://brew.sh/): `$> brew install --build-from-source opencv` will install required dependencies and compile opencv with the `opencv_contrib` module by default (no need to set `-DOPENCV_ENABLE_NONFREE=ON` manually). 
 * The OpenCV 4.1.0 source code can be found [here](https://github.com/opencv/opencv/tree/4.1.0)

4. gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using either [MinGW-w64](http://mingw-w64.org/doku.php/start) or [Microsoft's VCPKG, a C++ package manager](https://docs.microsoft.com/en-us/cpp/build/install-vcpkg?view=msvc-160&tabs=windows). VCPKG maintains its own binary distributions of OpenCV and many other packages. To see what packages are available, type `vcpkg search` at the command prompt. For example, once you've _VCPKG_ installed, you can install _OpenCV 4.1_ with the command:
```bash
c:\vcpkg> vcpkg install opencv4[nonfree,contrib]:x64-windows
```
Then, add *C:\vcpkg\installed\x64-windows\bin* and *C:\vcpkg\installed\x64-windows\debug\bin* to your user's _PATH_ variable. Also, set the _CMake Toolchain File_ to *c:\vcpkg\scripts\buildsystems\vcpkg.cmake*.


## Basic Build Instructions

1. Clone this repo.
2. Make a build directory in the top level directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./2D_feature_tracking`.

## Project Performance Evaluation
### MP.1 Data Buffer Optimization
For memory optimization instead of a `std::vector`, a `std::deque` is being used. 

![alt text][image1]

When an incoming images arrives and its keypoints are extracted the image is inserted in the buffer. When the buffer size is greater or equal to two we proceed with the keypoint matching. And when the buffer size is equal to max number of buffer size, then the head data will be deleted before the new data pushed into the tail of data buffer queue.

![alt text][image2]

### MP.2 Keypoint Detection
- SHITOMASI
![alt text][image8]
- HARRIS
![alt text][image6]
- FAST
![alt text][image5]
- BRISK
![alt text][image4]
- ORB
![alt text][image7]
- AKAZE
![alt text][image3]
- SIFT
![alt text][image9]

### MP.3 Keypoint Removal
Here is keypoint removal implementation
![alt text][image11]

### MP.4 Keypoint Descriptors
The following keypoint descriptors are implemented: BRISK, BRIEF, ORB, FREAK, AKAZE and SIFT. 

### MP.5 Descriptor Matching
The following matchers implemented in this project: FLANN and Brute Force. And the available selectors are: Nearest Neighbor and K-Nearest Neighbor (with the KNN being calibrated to two best matches).

![alt text][image12]

### MP.6 Descriptor Distance Ratio
In the implementation the threshold values is set to 0.8

![alt text][image15]

### MP.7 Performance Evaluation 1
In the table bellow we have the number of keypoints detected for the preceding vehicle for all 10 images and for all the detector types. And the three detectors that have returned the most keypoints are: BRISK, AKAZE and FAST.

Image index    |  SHITOMASI  |    HARRIS   |     FAST    |     BRISK   |     ORB     |    AKAZE    |   SIFT 
-------------  | :---------: | :---------: | :---------: | :---------: | :---------: | :---------: | :---------: 
Img. Id. 0     | 125         | 17          | 149         | 264         | 92          | 166         | 138         
Img. Id. 1     | 118         | 14          | 152         | 282         | 102         | 157         | 132         
Img. Id. 2     | 123         | 18          | 150         | 182         | 106         | 161         | 124         
Img. Id. 3     | 120         | 21          | 155         | 277         | 113         | 155         | 137         
Img. Id. 4     | 120         | 26          | 149         | 297         | 109         | 163         | 134        
Img. Id. 5     | 113         | 43          | 149         | 279         | 125         | 164         | 140         
Img. Id. 6     | 114         | 18          | 156         | 289         | 130         | 173         | 137         
Img. Id. 7     | 123         | 31          | 150         | 272         | 129         | 175         | 148         
Img. Id. 8     | 111         | 26          | 138         | 266         | 127         | 177         | 159         
Img. Id. 9     | 112         | 34          | 143         | 254         | 128         | 179         | 137         

### MP.8 Performance Evaluation 2
The below table shows the number of average matched keypoints for all 9 match instances using all possible detector-descriptor combinations. 

Descriptor/Detector Type  |     BRISK   |     BRIEF   |      ORB    |     FREAK   |     AKAZE   |  SIFT 
-------------             | :---------: | :---------: | :---------: | :---------: | :---------: | :---------: 
SHITOMASI                 | 85          | 104         | 101         | 85          | N.A.        | 118         
HARRIS                    | 16          | 19          | 18          | 16          | N.A.        | 18          
FAST                      | 86          | 98          | 95          | 73          | N.A.        | 116         
BRISK                     | 144         | 149         | 101         | 121         | N.A.        | 182         
ORB                       | 72          | 50          | 57          | 38          | N.A.        | 84         
AKAZE                     | 123         | 120         | 102         |107          | 130         | 141         
SIFT                      | 59          | 66          | N.A.        |138          | N.A.        | 138         

### MP.9 Performance Evaluation 3
The below table shows the average processing time for all detector-descriptor combination applied and averaged on all 10 input images. And the top 3 fastest detector-descriptor combinations are: FAST + BRIEF, FAST + ORB, FAST + BRISK. 

Descriptor/Detector Type  |     BRISK   |     BRIEF   |      ORB    |     FREAK   |     AKAZE   |  SIFT 
-------------             | :---------: | :---------: | :---------: | :---------: | :---------: | :---------: 
SHITOMASI                 | 18.2692     | 20.8301     | 23.7505     | 47.2676     | N.A.        | 38.3186         
HARRIS                    | 16.8764     | 20.0132     | 23.5778     | 46.1268     | N.A.        | 38.6313         
FAST                      | 2.5276      | 2.5742      | 5.5745      | 30.4137     | N.A.        | 19.0452         
BRISK                     | 36.7178     | 36.9879     | 44.8616     | 59.0667     | N.A.        | 53.8333         
ORB                       | 31.9859     | 32.0033     | 51.3630     | 60.1264     | N.A.        | 65.8400        
AKAZE                     | 66.3685     | 70.7968     | 75.4172     | 95.5817     | 130.7667    | 82.5465        
SIFT                      | 82.0138     | 99.4756     | N.A.        | 126.3014    | N.A.        | 137.5032     