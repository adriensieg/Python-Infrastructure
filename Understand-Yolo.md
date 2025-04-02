# Yolo, my big takeways

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

<img src="https://github.com/user-attachments/assets/bde33277-973d-4a16-a665-863a654ceb8a" width="50%" height="50%">

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

## YOLO - Architecture explanation

- YOLO makes use of only convolutional layers, making it a fully convolutional network (FCN).
- It has 75 convolutional layers, with skip connections and upsampling layers.
- No form of pooling is used, and a convolutional layer with stride 2 is used to downsample the feature maps. This helps in preventing loss of low-level features often attributed to pooling.
- Being a FCN, YOLO is invariant to the size of the input image.
- A big one amongst these problems is that if we want to process our images in batches (images in batches can be processed in parallel by the GPU, leading to speed boosts), we need to have all images of fixed height and width. This is needed to concatenate multiple images into a large batch (concatenating many PyTorch tensors into one)
- The network downsamples the image by a factor called the stride of the network.
  - For example, if the stride of the network is 32, then an input image of size 416 x 416 will yield an output of size 13 x 13. Generally, stride of any layer in the network is equal to the factor by which the output of the layer is smaller than the input image to the network.

- Typically, (as is the case for all object detectors) the features learned by the convolutional layers are passed onto a classifier/regressor which makes the detection prediction (coordinates of the bounding boxes, the class label.. etc).

- In YOLO, the prediction is done by using a convolutional layer which uses 1 x 1 convolutions.

- Now, the first thing to notice is our output is a feature map. Since we have used 1 x 1 convolutions, the size of the prediction map is exactly the size of the feature map before it. In YOLO v3 (and it's descendants), the way you interpret this prediction map is that each cell can predict a fixed number of bounding boxes.

<img src="https://github.com/user-attachments/assets/65027fb6-0dd7-4cfd-9821-f6ca3c4a8ca4" width="50%" height="50%">

https://dkharazi.github.io/notes/ml/cnn/yolo

A regressor rather than a classifier - https://christopher5106.github.io/object/detectors/2017/08/10/bounding-box-object-detectors-understanding-yolo.html

## Concepts to master

- Convolutional neural networks
- Residual Blocks
- Skip connections
- Upsampling / Downsampling
- Object detection
- Bounding box regression
- IoU
- Kernel vs. Filter
- **Non-Max Suppression (NMS)** to Eliminate Double Detections
To eliminate duplicates, we will use Non-Max Suppression (NMS). NMS evaluates the extent to which detections overlap using the Intersection over Union metric and, upon exceeding a defined threshold, treats them as duplicates. Duplicates are then discarded, starting with those of the lowest confidence. The value should be within the range [0, 1]. The smaller the value, the more restrictive the NMS.

- Depthwise vs. Pointwise

- Illustration of **Depthwise Convolution** (DWC)
<img src="https://github.com/user-attachments/assets/0ffa58ff-1c5c-4859-bafd-ddff86c2c69d" width=50% height=50%>

- Illustration of **Pointwise Convolution** (PWC)
<img src="https://github.com/user-attachments/assets/09bfd1b3-978e-41ea-aa8b-924e92c0045c" width=50% height=50%>

In standard convolutions, we are analyzing an **input map** of **height H** and **width W** comprised of **C channels**. To do so, we have a **squared kernel** of size **K** x **K** with typical values something like **3x3**, **5x5** or **7x7**. Moreover, we also specify **how many of such kernel features** we want to compute which is the number of **output channels O**.

<img src="https://github.com/user-attachments/assets/992f72ed-00fe-4a11-97e2-8b98002c500f" width=30% height=30%>

The input **feature map** is of size **W** x **H** and has **C channels** (here C=4). A kernel of size KxK is moved horizontally and vertically over the input feature map to compute the output for each location. 

The KxK kernel also covers each of the C channels. There are O of such kernels for each output feature to be computed (here O=8).

https://www.paepper.com/blog/posts/depthwise-separable-convolutions-in-pytorch/

```
```

## The Art of Possible
- **Time in Zone**: https://github.com/roboflow/supervision/tree/develop/examples/time_in_zone

- **Smart-Queue-Monitoring-System**: https://github.com/ObinnaIheanachor/Smart-Queue-Monitoring-System/tree/master?ref=blog.roboflow.com

- https://colab.research.google.com/drive/1eeYLjRWedIIcoHJJoAVinQt0GT-_fpzC?usp=sharing&ref=blog.roboflow.com#scrollTo=4hzCCJVcqw0r

- **How to Train YOLOv12 Object Detection on a Custom Dataset**: https://colab.research.google.com/github/roboflow-ai/notebooks/blob/main/notebooks/train-yolov12-object-detection-model.ipynb
  
- **Counting People On Escalator Using Yolov8 and OpenCV**: 
  - https://github.com/rahilmoosavi/CountingPeopleOnEscalatorUsingYolo/blob/master/PeopleCounter.py
  - https://medium.com/@rahil.gh.moosavi/counting-people-on-escalator-using-yolov8-and-opencv-from-scratch-1da725c0df66
 
- **How to Detect and Count Objects in Zone**: https://github.com/roboflow/notebooks/blob/main/notebooks/how-to-detect-and-count-objects-in-polygon-zone.ipynb

- **Object-Detection-and-Count-in-polygon-zone**: https://github.com/noorkhokhar99/Object-Detection-and-Count-in-polygon-zone/tree/main

- **Yolov8-Counting-People-in-Queue**: https://github.com/freedomwebtech/Yolov8-Counting-People-in-Queue/tree/main  
  - https://github.com/freedomwebtech/Yolov8-Counting-People-in-Queue/blob/main/yolov8_object_detection_on_custom_dataset.ipynb

- **Real world applications from Ultralytics**: https://docs.ultralytics.com/guides/region-counting/#real-world-applications

- **Roboflow's notebooks (~50)**: https://github.com/roboflow/notebooks/tree/main/notebooks
  - https://github.com/roboflow/notebooks/blob/main/notebooks/how-to-track-and-count-vehicles-with-yolov8-and-supervison.ipynb
  - https://github.com/roboflow/notebooks/blob/main/notebooks/how-to-track-and-count-vehicles-with-yolov8.ipynb
  - **Zero-shot object detection**: https://github.com/roboflow/notebooks/blob/main/notebooks/zero-shot-object-detection-with-yolo-world.ipynb
  - **Bounding Box to Semantic Mask**: https://github.com/roboflow/notebooks/blob/main/notebooks/how-to-use-yolo8v-with-sam.ipynb
    - **... with videos**: https://github.com/roboflow/notebooks/blob/main/notebooks/how-to-segment-videos-with-sam-2.ipynb

<img src="https://github.com/user-attachments/assets/df822af3-067e-46b1-8813-84a77ff2929d" width="20%" height="20%">

- **Detectron 2** - https://colab.research.google.com/drive/16jcaJoc6bCFAQ96jDe2HwtXj7BMD_-m5?authuser=1#scrollTo=EHr2WJXrV6Wp
  
https://medium.com/towards-data-science/types-of-convolution-kernels-simplified-f040cb307c37

https://albumentations.ai/docs/getting_started/bounding_boxes_augmentation/

https://github.com/roboflow/supervision?tab=readme-ov-file

<img src="https://github.com/user-attachments/assets/2402a9ca-13ba-4bb3-b939-9e7a761664ec" width="20%" height="20%">

https://colab.research.google.com/drive/16jcaJoc6bCFAQ96jDe2HwtXj7BMD_-m5?authuser=1#scrollTo=h9tECBQCvMv3

- DIY Auto Tracking Pan Tilt Camera For Raspberry Pi and SBCs : https://www.youtube.com/watch?v=1lxTk2OjOdk

- https://www.digitalocean.com/community/tutorials/train-yolov5-custom-data

## Intersection over Union

https://viso.ai/computer-vision/intersection-over-union-iou/

Is the ratio of the **‘area of intersection’** to the **‘area of the union’** between the **predicted** and **ground truth bounding boxes**. 
Thus, the IoU meaning consists of the quantitative measurement of **how well a predicted bounding box** aligns with **the ground truth bounding box**.

<img src="https://github.com/user-attachments/assets/e0a15606-f9ae-4398-8c02-4bada592eb17" width="50%" height="50%">

- **Area of Intersection** = Common area shared by the two bounding boxes (Overlap)
- **Area of Union** = Total area covered by the two bounding boxes
- **Ground Truth Bounding Box** = Defines the exact location and size of an object in an image and serves as the reference point for evaluating the model’s predictions
- **Predicted Bounding Box**
- **Overlap**

<img src="https://github.com/user-attachments/assets/1511b81c-222c-429f-a0dd-9cf463ed8f48" width="50%" height="50%">

#### How is IoU Calculated?

<img src="https://github.com/user-attachments/assets/dc3b4ba3-d695-4cac-aa01-8b8342a7cbb1" width="50%" height="50%">

#### Understanding Box (P, R, mAP50, mAP50-95) Metrics in Object Detection

Object detection models, metrics like **Precision (P)**, **Recall (R)**, **mAP50**, and **mAP50-95** play a crucial role in assessing **how well the model identifies and classifies objects in images**. 

These metrics help measure **accuracy**, **coverage**, and **robustness** across different scenarios.

- **P (Precision)**: Measures the proportion of correct positive predictions out of all predicted positives. A high precision means fewer false positives, which is crucial when false detections can have significant consequences (e.g., medical imaging or security applications).

Formula: $P = \frac{TP}{TP + FP}$

- **R (Recall)**: Measures how well the model captures actual positive instances. A high recall indicates that the model is detecting most relevant objects but might also include some incorrect ones.

Formula: $R = \frac{TP}{TP + FN}$

- **mAP50 (Mean Average Precision at 50% IoU)**: This metric calculates the average precision at a single IoU threshold of 50%, meaning a predicted box must overlap at least 50% with the ground truth to be considered correct. It’s a commonly used benchmark for object detection performance.

- **mAP50-95 (Mean Average Precision across multiple IoU thresholds)**: This metric averages precision over IoU thresholds ranging from 50% to 95% in 5% increments (i.e., 50%, 55%, …, 95%). It provides a more comprehensive assessment of model performance, considering how well the model detects objects at varying levels of strictness.

## Fine-tune or Retrain

<img src="https://github.com/user-attachments/assets/753b0e0f-3499-4b8c-a99e-cb52f7d144d7" width="50%" height="50%">

#### Epoch:
  - The current epoch out of the total number of training epochs (e.g., 1/100 means the first epoch out of 100).
  - More epochs generally improve the model, but too many can cause overfitting.


#### GPU_mem: 
- The amount of GPU memory used during training (e.g., 6.74G means 6.74 GB of GPU memory usage).

#### box_loss: 
The loss associated with the bounding box regression, which measures how well the predicted bounding boxes align with ground-truth boxes.

#### cls_loss:
The classification loss, which measures how well the model is distinguishing between different object classes.

#### dfl_loss: 
The distribution focal loss, which helps refine the bounding box predictions.

Instances: The number of object instances processed in the current batch.
Size: The input image size used during training (e.g., 800 means images are resized to 800 pixels).
Validation Metrics
Class: The category of objects being evaluated (here, all refers to all classes combined).
Images: The number of images used for validation (e.g., 44 means 44 validation images).
Instances: The total number of object instances in the validation set.
Box(P): Precision for bounding box detection, measuring how many detected objects are correct.
R (Recall): The recall for bounding box detection, measuring how many actual objects were detected.
mAP50: Mean Average Precision at IoU=0.50, a standard object detection metric.
mAP50-95: Mean Average Precision across multiple IoU thresholds (0.50 to 0.95), a stricter and more comprehensive evaluation metric.


