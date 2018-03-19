# CSK Tracker in Python

<p align="center">
    <img src="https://github.com/hsjeong5/CSK/blob/master/img/dog1_1.jpg" width="32%">
    <img src="https://github.com/hsjeong5/CSK/blob/master/img/sylvester_1.jpg" width="32%">
    <img src="https://github.com/hsjeong5/CSK/blob/master/img/faceocc2_1.jpg" width="32%">
</p>

This is a Python implementation of "Exploiting the Circulant Structure of Tracking-by-Detection with Kernels" [1]. The above three sequences (Dog1, Sylvester, FaceOcc2) are taken from [2].

## Requirements

- Numpy
- Scipy

## Sample Code

```python
from csk import *
import numpy as np
from scipy.misc import imread, imsave
import cv2 # (Optional) OpenCV for drawing bounding boxes

length = 472 # sequence length

# 1st frame's groundtruth information
x1 = 125 # position x of the top-left corner of bounding box
y1 = 162 # position y of the top-left corner of bounding box
width = 74 # the width of bounding box
height = 55 # the height of bounding box

sequence_path = "" # your sequence path
save_path = "" # your save path

tracker = csk.CSK() # CSK instance

for i in range(1,length+1): # repeat for all frames
    frame = imread(sequence_path+"%04d.jpg"%i)

    if i == 1: # 1st frame
        print(str(i)+"/"+str(length)) # progress
        tracker.init(frame,x1,y1,width,height) # initialize CSK tracker with GT bounding box
        imsave(save_path+'%04d.jpg'%i,cv2.rectangle(cv2.cvtColor(frame, cv2.COLOR_GRAY2BGR), \
        (x1, y1), (x1+width, y1+height), (0,255,0), 2)) # draw bounding box and save the frame

    else: # other frames
        print(str(i)+"/"+str(length)) # progress
        x1, y1 = tracker.update(frame) # update CSK tracker and output estimated position
        imsave(save_path+'%04d.jpg'%i,cv2.rectangle(cv2.cvtColor(frame, cv2.COLOR_GRAY2BGR), \
        (x1, y1), (x1+width, y1+height), (0,255,0), 2)) # draw bounding box and save the frame
```

## Precision Plots

<p align="center">
  <img src="https://github.com/hsjeong5/CSK/blob/master/img/dog1_precision.png" width="32%">
  <img src="https://github.com/hsjeong5/CSK/blob/master/img/sylvester_precision.png" width="32%">
  <img src="https://github.com/hsjeong5/CSK/blob/master/img/faceocc2_precision.png" width="32%">
</p>

This is precision plots for 3 sequences above (Dog1, Sylvester, FaceOcc2).


## References
[1]
>[Exploiting the Circulant Structure of Tracking-by-Detection with Kernels](https://link.springer.com/chapter/10.1007/978-3-642-33765-9_50)<br>
> João F. Henriques, Rui Caseiro, Pedro Martins, Jorge Batista<br>
> ECCV 2012

[2]
>[Online Object Tracking: A Benchmark](http://cvlab.hanyang.ac.kr/tracker_benchmark/index.html)<br>
> Yi Wu, Jongwoo Lim, Ming-Hsuan Yang<br>
> CVPR 2013
