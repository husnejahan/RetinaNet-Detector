# RetinaNet-Detector
RetinaNet Detector

# Introduction to Object Detection
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


# Class Imbalance Problem of One-Stage Detector

- A much larger set of candidate object locations is regularly sampled across an image (~100k locations), which densely cover spatial positions, scales and aspect ratios.
- The training procedure is still dominated by easily classified background examples. It is typically addressed via bootstrapping or hard example mining. But they are not efficient enough.
![GitHub Logo](/images/ssd.png)

# RetinaNet
RetinaNet uses ResNet-101-FPN as backbone architecture and two-task specific subnetworks(classification and localization). It is combination of anchors used in all previous architectures and feature pyramids used in SSD.
A new loss function Focal loss to deal with the foreground-background class imbalance posed in one-stage detectors.
![GitHub Logo](/images/retinanet.png)

RetinaNet is a single, unified network composed of a backbone network and two task-specific subnetworks. The backbone is responsible for computing a convolutional fea- ture map over an entire input image and is an off-the-self convolutional network. The first subnet performs convo- lutional object classification on the backbone’s output; the second subnet performs convolutional bounding box regres- sion. The two subnetworks feature a simple design that we propose specifically for one-stage, dense detection

# Focal Loss
- Predicting the class of the object (n class probabilities) is a classification problem. 
- Predicting the four coordinates for the bounding box is a regression problem. We need a loss function that combines these two problems.
- The loss is a weighted sum of localization loss (bounding boxes) and confidence loss (classes): We calculate how much the predicted bounding box (default box coordinates + predicted offsets) differs from the matched ground truth bounding box (L1 loss) and how correctly the default box predicted the class (binary cross entropy).

- The class imbalance causes two problems: (1) training is inefficient as most locations are easy negatives that contribute no useful learning signal; (2) the easy negatives can overwhelm training and lead to degenerate models.

 
