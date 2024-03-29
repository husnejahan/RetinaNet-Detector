# RetinaNet-Detector
[RetinaNet Detector](https://towardsdatascience.com/review-retinanet-focal-loss-object-detection-38fba6afabe4)

# [Introduction to Object Detection](https://dudeperf3ct.github.io/object/detection/2019/01/07/Mystery-of-Object-Detection/#classification-loss)
Here, the classification and localization task contains input single image and we are supposed to identify what class does that image belong to and where is it. But in object detection, this problem gets blown on a multiple scale. There can be any number of objects in image and each object will have different size in image, for given image we have to detect the category the object belong to and locate the object.

![GitHub Logo](/images/puppy.png)

# SSD

- Pass the image through a series of convolutional layers, yielding several sets of feature maps at different scales (e.g. 10x10, then 6x6, then 3x3, etc.)
- For each location in each of these feature maps, use a 3x3 convolutional filter to evaluate a small set of default bounding boxes. These default bounding boxes are essentially equivalent to Faster R-CNN’s anchor boxes
- For each box, simultaneously predict a) the bounding box offset and b) the class probabilities
During training, match the ground truth box with these predicted boxes based on IoU. The best predicted box will be labeled a “positive,” along with all other boxes that have an IoU with the truth >0.5
- To put it simply, SSD approach is based on a feed-forward convolutional network that produces a fixed-size collection of bounding boxes and scores for the presence of object class instances in those boxes, followed by a non-maximum suppression step to produce the final detections.
![GitHub Logo](/images/ssd1.jpeg)

# YOLOv3
- New CNN architecutre with 53 layers, or popularly known among dark side as Darknet-53
- Replace softmax with independent logistic classifiers. Using a softmax imposes the assumption that each box has exactly one class which is often not the case. A multilabel approach better models the data.

- YOLOv3 predicts boxes at 3 different scales and extracts features from those scales using a similar concept to feature pyramid networks. Model predicts a 3-d tensor encoding bounding box, objectness, and class predictions. For e.g. N x N x (3 * (4 + 1 + C)) for the 4 bounding box offsets, 1 objectness prediction, and C class predictions.

- Choose 9 clusters and 3 scales arbitrarily and then divide up the clusters evenly across scales. For e.g. (10 x 13), (16 x 30), (33 x 23), (30 x 61), (62 x 45), (59 x 119), (116 x 90), (156 x 198), (373 x 326).
![GitHub Logo](/images/yolo_v3.png)

# Hard example mining
For most detectors like SSD and YOLO, we make far more predictions than the number of objects presence. So there are much more negative matches than positive matches. This creates a class imbalance which hurts training. We are training the model to learn background space rather than detecting objects. However, we need negative sampling so it can learn what constitutes a bad prediction. So, for example in SSD, we sort training examples by their calculated confidence loss. We pick the top ones and makes sure the ratio between the picked negatives and positives is at most 3:1. This leads to a faster and more stable training.
# Non-maximal suppression in inference
Detectors can make duplicate detections for the same object. To fix this, we apply non-maximal suppression to remove duplications with lower confidence. We sort the predictions by the confidence scores and go down the list one by one. If any previous prediction has the same class and IoU greater than 0.5 with the current prediction, we remove it from the list.

# Class Imbalance Problem of One-Stage Detector

- A much larger set of candidate object locations is regularly sampled across an image (~100k locations), which densely cover spatial positions, scales and aspect ratios.
- The training procedure is still dominated by easily classified background examples. It is typically addressed via bootstrapping or hard example mining. But they are not efficient enough.
![GitHub Logo](/images/ssd.png)

# RetinaNet
RetinaNet uses ResNet-101-FPN as backbone architecture and two-task specific subnetworks(classification and localization). It is combination of anchors used in all previous architectures and feature pyramids used in SSD.
A new loss function Focal loss to deal with the foreground-background class imbalance posed in one-stage detectors.
![GitHub Logo](/images/retinanet.png)

- RetinaNet forms a single FCN comprised of a ResNet-FPN backbone, a classification subnet, and a box regression subnet
- RetinaNet is a single, unified network composed of a backbone network and two task-specific subnetworks. The backbone is responsible for computing a convolutional feature map over an entire input image and is an off-the-self convolutional network. 
- The first subnet performs convolutional object classification on the backbone’s output; the second subnet performs convolutional bounding box regression. The two subnetworks feature a simple design that we propose specifically for one-stage, dense detection
- we propose the focal loss which applies a modulating term to the cross entropy loss in order to focus learning on hard negative examples
- ResNet is used for deep feature extraction.
- Feature Pyramid Network (FPN) is used on top of ResNet for constructing a rich multi-scale feature pyramid from one single resolution input image. (Originally, FPN is a two-stage detector which has state-of-the-art results. Please read my review about FPN if interested.)
- FPN is multiscale, semantically strong at all scales, and fast to compute.
There are some modest changes for the FPN here. A pyramid is generated from P3 to P7. Some major changes are: P2 is not used now due to computational reasons. (ii) P6 is computed by strided convolution instead of downsampling. (iii) P7 is included additionally to improve the accuracy of large object detection.

# Focal Loss
- Predicting the class of the object (n class probabilities) is a classification problem. 
- Predicting the four coordinates for the bounding box is a regression problem. We need a loss function that combines these two problems.
- The loss is a weighted sum of localization loss (bounding boxes) and confidence loss (classes): We calculate how much the predicted bounding box (default box coordinates + predicted offsets) differs from the matched ground truth bounding box (L1 loss) and how correctly the default box predicted the class (binary cross entropy).
In practice we use an alpha balanced variant of the focal loss.

- The class imbalance causes two problems: (1) training is inefficient as most locations are easy negatives that contribute no useful learning signal; (2) the easy negatives can overwhelm training and lead to degenerate models.

- Cross Entropy (CE) Loss

![GitHub Logo](/images/ce.png)

- α-Balanced CE Loss

 ![GitHub Logo](/images/ace.png)
 
 - Focal Loss (FL)
 
 ![GitHub Logo](/images/fl.png)
 
 - α-Balanced Variant of FL
 
 ![GitHub Logo](/images/afl.png)
 
[retinanet](https://towardsdatascience.com/review-retinanet-focal-loss-object-detection-38fba6afabe4)

