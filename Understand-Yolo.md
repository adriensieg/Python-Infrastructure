## Concepts to master

- Convolutional neural networks
- Residual Blocks
- Skip connections
- Upsampling / Downsampling
- Object detection
- Bounding box regression
- IoU
- Non-maximum suppression
- Kernel vs. Filter

- Depthwise vs. Pointwise

- Illustration of **Depthwise Convolution** (DWC)
<img src="https://github.com/user-attachments/assets/0ffa58ff-1c5c-4859-bafd-ddff86c2c69d" width=50% height=50%>

- Illustration of **Pointwise Convolution** (PWC)
<img src="https://github.com/user-attachments/assets/09bfd1b3-978e-41ea-aa8b-924e92c0045c" width=50% height=50%>

In standard convolutions, we are analyzing an **input map** of **height H** and **width W** comprised of **C channels**. To do so, we have a **squared kernel** of size **K** x **K** with typical values something like **3x3**, **5x5** or **7x7**. Moreover, we also specify **how many of such kernel features** we want to compute which is the number of **output channels O**.

<img src="https://github.com/user-attachments/assets/992f72ed-00fe-4a11-97e2-8b98002c500f" width=50% height=50%>

The input **feature map** is of size **W** x **H** and has **C channels** (here C=4). A kernel of size KxK is moved horizontally and vertically over the input feature map to compute the output for each location. 

The KxK kernel also covers each of the C channels. There are O of such kernels for each output feature to be computed (here O=8).

https://www.paepper.com/blog/posts/depthwise-separable-convolutions-in-pytorch/

## YOLO - Architecture explanation

YOLO makes use of only convolutional layers, making it a fully convolutional network (FCN). It has 75 convolutional layers, with skip connections and upsampling layers. 
No form of pooling is used, and a convolutional layer with stride 2 is used to downsample the feature maps. This helps in preventing loss of low-level features often attributed to pooling.
Being a FCN, YOLO is invariant to the size of the input image.
A big one amongst these problems is that if we want to process our images in batches (images in batches can be processed in parallel by the GPU, leading to speed boosts), we need to have all images of fixed height and width. This is needed to concatenate multiple images into a large batch (concatenating many PyTorch tensors into one)
The network downsamples the image by a factor called the stride of the network. For example, if the stride of the network is 32, then an input image of size 416 x 416 will yield an output of size 13 x 13. Generally, stride of any layer in the network is equal to the factor by which the output of the layer is smaller than the input image to the network.
Typically, (as is the case for all object detectors) the features learned by the convolutional layers are passed onto a classifier/regressor which makes the detection prediction (coordinates of the bounding boxes, the class label.. etc).
In YOLO, the prediction is done by using a convolutional layer which uses 1 x 1 convolutions.
Now, the first thing to notice is our output is a feature map. Since we have used 1 x 1 convolutions, the size of the prediction map is exactly the size of the feature map before it. In YOLO v3 (and it's descendants), the way you interpret this prediction map is that each cell can predict a fixed number of bounding boxes.

![image](https://github.com/user-attachments/assets/65027fb6-0dd7-4cfd-9821-f6ca3c4a8ca4)
https://dkharazi.github.io/notes/ml/cnn/yolo

A regressor rather than a classifier - https://christopher5106.github.io/object/detectors/2017/08/10/bounding-box-object-detectors-understanding-yolo.html

## Understand the Architecture

- The input image has dimensions **(448×448×3)**, meaning it has **height = 448**, **width = 448**, and **3 color channels** (RGB).
- The network applies **multiple convolutional layers**, interleaved with **pooling layers**, which **progressively reduce spatial dimensions** while **increasing depth**.
- The final layer outputs a **(7×7×30) tensor**, **representing bounding box predictions**.
- Interpreting the Final Output **(7×7×30)**
  - The final output is a **7×7 grid**, meaning the **image** is divided into **49 cells**.
  - Each cell predicts:
    - **5 bounding boxes** (each with **5 values**: **x**, **y**, **w**, **h**, **confidence**) → **(5×2 = 10) values**.
    - **Class probabilities** for **20 classes** (assuming Pascal VOC dataset).
    - **Total output** per cell: **30 values** → So **final tensor** is **(7×7×30)**.
   
    - The image is progressively **downsampled** in **height** & **width** while **increasing in depth**.
    - The final **7×7×30 output** is a compact representation of object detections.
    - Bounding box predictions and **class probabilities** are extracted from this final tensor.

![image](https://github.com/user-attachments/assets/bde33277-973d-4a16-a665-863a654ceb8a)

```
| Layer Type          | Filter Size (K) | Stride (S) | Input Size (H×W×D) | Output Size (H×W×D) | Description |
|---------------------|---------------|-----------|----------------|----------------|-------------|
| **Input Image**     | -             | -         | 448×448×3      | 448×448×3      | Original RGB image |
| **Conv Layer 1**    | 7×7×64        | 2         | 448×448×3      | 224×224×64     | Large receptive field, extracts basic features |
| **MaxPool 1**       | 2×2           | 2         | 224×224×64     | 112×112×64     | Reduces spatial size |
| **Conv Layer 2**    | 3×3×192       | 1         | 112×112×64     | 112×112×192    | Extracts more complex features |
| **MaxPool 2**       | 2×2           | 2         | 112×112×192    | 56×56×192      | Downsampling |
| **Conv Layer 3**    | 1×1×128       | 1         | 56×56×192      | 56×56×128      | Bottleneck (reduces depth before next conv layer) |
| **Conv Layer 4**    | 3×3×256       | 1         | 56×56×128      | 56×56×256      | Extracts detailed features |
| **Conv Layer 5**    | 1×1×256       | 1         | 56×56×256      | 56×56×256      | Another bottleneck |
| **Conv Layer 6**    | 3×3×512       | 1         | 56×56×256      | 56×56×512      | Deeper features |
| **MaxPool 3**       | 2×2           | 2         | 56×56×512      | 28×28×512      | Downsampling |
| **Conv Layers 7-10**| 1×1, 3×3 alternations | 1 | 28×28×512 | 28×28×1024 | 4 alternating layers refining features |
| **MaxPool 4**       | 2×2           | 2         | 28×28×1024     | 14×14×1024     | Downsampling |
| **Conv Layers 11-14** | 1×1, 3×3 alternations | 1 | 14×14×1024 | 14×14×1024 | Further feature extraction |
| **MaxPool 5**       | 2×2           | 2         | 14×14×1024     | 7×7×1024       | Downsampling |
| **Conv Layers 15-16** | 3×3×1024      | 1         | 7×7×1024       | 7×7×1024       | Final convolutional feature extraction |
| **Fully Connected** | -             | -         | 7×7×1024       | 4096           | Flatten and dense connections |
| **Fully Connected** | -             | -         | 4096           | 7×7×30         | Final output layer (grid cells with bounding boxes) |
```

## The Art of Possible
https://github.com/roboflow/supervision/tree/develop/examples/time_in_zone


## Ressources

https://medium.com/towards-data-science/types-of-convolution-kernels-simplified-f040cb307c37
Counting People On Escalator Using Yolov8 and OpenCV: 
- https://github.com/rahilmoosavi/CountingPeopleOnEscalatorUsingYolo/blob/master/PeopleCounter.py
- https://medium.com/@rahil.gh.moosavi/counting-people-on-escalator-using-yolov8-and-opencv-from-scratch-1da725c0df66

https://albumentations.ai/docs/getting_started/bounding_boxes_augmentation/

https://github.com/roboflow/supervision?tab=readme-ov-file

Detectron 2 - https://colab.research.google.com/drive/16jcaJoc6bCFAQ96jDe2HwtXj7BMD_-m5?authuser=1#scrollTo=EHr2WJXrV6Wp

https://user-images.githubusercontent.com/11255376/71348563-272db480-25b0-11ea-8530-67fff677f551.gif
https://github.com/imsoo/darknet_server?tab=readme-ov-file

https://colab.research.google.com/drive/16jcaJoc6bCFAQ96jDe2HwtXj7BMD_-m5?authuser=1#scrollTo=h9tECBQCvMv3
