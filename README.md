# Cigarette Detection

<!-- TOC -->

- [Building on Linux](#building-on-linux)
- [Dataset](#dataset)
- [Preparation](#preparation)
- [Results](#results)

<!-- /TOC -->

### Building on Linux
We are going to follow the instructions given [here](https://pythonprogramming.net/haar-cascade-object-detection-python-opencv-tutorial/):

To build you need [CMake](http://www.cmake.org/) and [OpenCV](http://opencv.org/) on your system.
```bash
$ git clone https://github.com/Itseez/opencv.git
```
Compiler: 
```bash
sudo apt-get install build-essential
```
Required Libraries:
```bash
$ sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
```
Python bindings and such: 
```bash
$ sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev
```
OpenCV development library:
```bash
sudo apt-get install libopencv-dev
```

### Dataset
As **positive image**, I tried both training the haar cascade with a cigarette image (cigar100x75.jpg) and a hand image holding a cigarette (cigar100x84.jpg).
As **negative image**, I took a sample of 5394 images from a face dataset.

### Preparation
We need to create files which contains information about our dataset. 

Run ***create_neg()*** and ***create_pos()*** functions in the *sample_creator.ipynb* notebook to create those files named as "bg.txt" which contains the paths of negative images and "info.dat" which contains the coordinates of the cigarette object in our positive image.

After that, run the following commands in terminal to create positive samples:
```bash
opencv_createsamples -img cigar100x84.jpg -bg bg.txt -info positive_images/info.lst -pngout positive_images -maxxangle 0.5 -maxyangle 0.5 -maxzangle 0.5 -num 1950
```
Once the positive samples have been created, we need a new file which contains information stating that where the cigarette is in our recently created positive images, just like "info.dat". We will call it as "info.lst"
```bash
opencv_createsamples -info positive_images/info.lst -num 1950 -w 50 -h 40 -vec positives.vec
```
Now, it's time to train a haar cascade to detect cigarettes in images and hopefully in videos.
```bash
opencv_traincascade -data data -vec positives.vec -bg bg.txt -numPos 1900 -numNeg 950 -numStages 6 -w 50 -h 40 
```
Done!

Now we can try our cascade using the codes in the ***detect_my_cigar.ipynb*** notebook.

### Results
<img src="https://user-images.githubusercontent.com/36932448/67939156-01081b80-fbe2-11e9-9cbf-91e728a518af.jpeg"
     alt="Markdown Monster icon" 
     width=720; height=480/>
<img src="https://user-images.githubusercontent.com/36932448/67939157-01081b80-fbe2-11e9-87d7-db891561edf8.jpeg"
     alt="Markdown Monster icon" 
     width=720; height=480/>
<img src="https://user-images.githubusercontent.com/36932448/67939159-01a0b200-fbe2-11e9-9099-53e60057db76.jpeg"
     alt="Markdown Monster icon" 
     width=720; height=480/>
