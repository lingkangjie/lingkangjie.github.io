---
layout:     post
title:      "Notes for A Survey of Object detection"
date:       2019-05-28 12:00:00
author:     "Ling"
header-img: "img/post-bg-2015.jpg"
tags:
    - Deep Learning
    - Machine Learning
    - Object detection
---

## Object Detection in 20 years: A Survey
> authors: Zhengxia Zou, Zhenwei Shi, Yuhong Guo, et al.. Notes by Lingkangjie, 2019-06-05

**Abstract**: Introduce the milestone detectors in history, detection datasets, metrics, fundamental building blocks of the detection system, speed up techniques, and state-of-the-art detection methods, and applications, such as pedestrian detection, face detection, text detection, etc.

### Introduction
- Object detection techniques form the basis of many other computer vision task, such as instance segmentation, image captioning, object tracking, etc. Object detection has now been widely used in many real-world applications, such as autonomous driving, robot vision, video, surveillance, etc.
- Milestone detectors: VJ detector (2001), HOG detector (2005), DPM + Bounding box regression (2008,2010), two-stage detector {RCNN (2014), SPPNet (2014), Fast RCNN (2015), Pyramid Networks (2017)}, one-state detector {YOLO (2016,2017), SSD (2016), Retina-Net (2017)}
- What are the difficulties and challenges in object detection? Some Common challenges includes objects under different viewpoints, illuminations, intraclass variations, object rotation, scale changes(e.g., small objects), accurate object localization, dense and occluded object detection, speed up of detection, etc. As different detection tasks have totally different objectives and constraints, their difficulties vary from each other.

### Object detection in 20 years

#### Detectors
- VJ detector introduced three important techniques: integral image, feature selection, detection cascades.
- HOG detector introduces multiple times image scaling technique while keeping the size of detection window unchanged. HOG detector was motivated primarily by the problem of pedestrian detection.
- Deformable Part-based Model (DPM), is the winners of VOC-07, -08, and -09 detection challenges. The key idea behind DPM is "divide and conquer", where the training can be simply considered as the learning of a proper way of decomposing an object, and the inference can be considered as an ensemble of detections on different object parts. A typical DPM detector consists of a root-filter and a number of part-filters. Later, some new ideas merged into DPM such as mixture models, hard negative mining, bounding box regression, etc.
- The key idea behind RCNN is simple: get object proposals by selective search, and each proposal is rescaled to a fixed size image and fed into a CNN model trained on ImageNet to extract features, finally use linear SVM classifiers to classify. There are total 2000 boxes from one image.
- SPPNet introduces a Spatial Pyramid Pooling (SPP) layer, which enable a CNN to generate a fixed-length representation regardless of the size of image/region of interest without rescaling it.
- Inspired by R-CNN and SPPNet, a new network called Fast RCNN which enables us to simultaneously train a detector and a bounding box regressor under the same network configurations was proposed. Fast RCNN still uses proposal detection technique.
- "Can we generate object proposals with a CNN model", So Faster RCNN came into being. The main contribution of Faster-RCNN is the introduction of Region Proposal Network (RPN) that enables nearly cost-free region proposals. 
- Most individual blocks of an object detection system, e.g., proposal detection, feature extraction, bounding box regression, etc, have been gradually integrated into a unified, end-to-end learning framework called one-stage detection. 
- You Only Look Once (YOLO), divides the image into regions and predicts bounding boxes and probabilities for each region simultaneously. YOLO suffers from a drop of the localization accuracy compared with two-stage detectors, especially for some small objects. As YOLO use pre-defined anchors to locate an object.
- Single Shot MultiBox Detector (SSD). The main contribution of SSD is the introduction of the multi-reference and multi-resolution detection techniques. Any previous detectors detect objects of different scales on different layers of the network (e.g, YOLO has three heads to detect three kinds of scaling), and SSD only runs detection on their top layers.
- RetinaNet introduces a new loss function suited for one-stage detection called "focal loss". Focal loss enables the one-stage detectors to achieve comparable accuracy of two-stage detectors while maintaining very high detection speed.

#### Datasets
- Pascal VOC (PASCAL Visual Object Classes), tasks including image classification, object detection, semantic segmentation and action detection. 
- ILSVRC (ImageNet Large Scale Visual Recognition Challenge)

```
VOC07: 5k training images, 12k annotated objects, 20 classes.
VOC12: 11k training images, 27k annotated objects, 20 classes.
ILSVRC: 517k images, 534k annotated objects, 200 classes.
MS-COCO-17: 164k images, 897k annotated objects, 80 classes.
OID (Open Images Detection): 1910k images, 15440k annotated objects, 600 categories.
```
- Object detection tasks: pedestrian detection, remote sensing target detection, face detection, text detection, traffic sign/light detection, etc.

#### Metrics
- In the early time, use "miss rate vs. false positives per-window (FPPW)". From VOC2007, introduces Average Precision (AP) and mean AP (mAP). MS-COCO AP is averaged over multiple IoU thresholds between 0.5 (coarse localization) and 0.95 (perfect localization).

#### Technical Evolution in Object Detection
- Early time, Recognition-by-components is the core idea of image recognition and object detection. The researchers propose rule-based or template based approaches by comparing the similarity between the object components, shapes and contours. In 1998-2005, people use wavelet feature representations to do object detection by transforming an image from pixels to a set of wavelet coefficients. Later (2005-2012), gradient-based representations of statistical model have been popular.
- The history of using CNN to detecting objects can be traced back to 1990s. In the early day, some Y. LeCun et al. works can be considered as the prototype of today's fuuly convolutional networks (FCN).
- Technical evolution of multi-scale detection:

```
T1: feature pyramids and silding windows (VJ Det., HOG, DPM, etc.)
T2: detection with object proposals (RCNN, SPPNet, Fast RCNN, Faster RCNN)
T3: deep regression (YOLO)
T4: multi-reference detection (SSD, YOLOv2)
T5: multi-resolution detection (FPN, RetinaNet)

years: --2001-...-2010--2011--2012--2013---2014---2015---2016---2017---2018---2019
          ------------ T1 ------------------>|
                   |<------------- T2------------->|
                                     |<--------- T3 ----->|
                                                   |<------- T4--------
                                                          |<------ T5----------
```
   - To answer the question " is there a unified multi-scale approach to detect objects of different aspect ratios?", then researchers introduce "object proposals" (T2). 
   - Pros of T3: simple, easy. Cons of T3: the localization may not be accurate enough especially for some small objects (T4 has latter solved this problem).
   - Multi-reference/Multi-resolution detection. The main idea of T4 is to pre-define a set of reference boxes (anchor boxes) with different sizes and aspect-ratios at different locations of an image, and then predict the detection box based on these reference. T5 is a technique to detect objects of different scales at different layers of the network. T5 Detects larger objects in deeper layers and smaller ones in shallower layers.

- Technical evolution of bounding box regression. BB aims to refine the location of a predicted bounding box based on the initial proposal or the anchor box.

```
T1: without bounding box regression(VJ Det., HOG, etc.)
T2: from bounding box to bounding box (DPM)
T3: from feature to bounding box (SPPNet, Fast RCNN, 
    Faster RCNN, YOLO, SSD, FPN, RetinaNet, TridentNet, etc.)

years: --2001-........--2008---------------2013---2016---2017---2018---2019
          ---- T1 ------->|
                          |<----- T2-------->|
                                                   |<--------- T3 -------
```
- Technical evolution of context priming. Visual objects are usually embedded in a typical context with the surrounding environments. We use the associations among objects and environments to perform object detection. This kind of method called context priming. 
   - Detection with local context. Some researchers found that by incorporating a small amount of background information improves the accuracy of pedestrian detection early in 2005. 
   - Detection with global context. The first way is to take advantage of large receptive field (even larger than the input image) or global pooling operation of a CNN feature. The second way is to think of the global context as a kind of sequential information such as in video, and learn it with recurrent neural networks.
   - Context interactive. Context interactive refers to the piece of information that conveys by the interactions of visual elements, such as the constrains and dependencies.
- Technical evolution of non-maximum suppression. NMS has been gradually developed into three groups of methods:
  - traditional greedy selection ( DPM, RCNN, Fast RCNN, Faster RCNN, YOLO, SSD, FPN, RetinaNet etc.): for a set of overlapped detections, the bounding box with the maximum detection score is selected while its neighoring boxes are removed according to a predefined overlap threshold (say, 0.5). Cons of this method: the top-scoring box may not be the best fit, it may suppress nearby objects, it does not suppress false positives.
  - bounding box aggregation: takes full consideration of object relationships and their spatial layout.
  - learning to NMS: think of NMS as a filter to re-score all raw detections and to train the NMS as part of a network in an end-to-end fashion.

![evolution of non-max suppresion](./imgs/NMS.jpg)

- Technical evolution of hard negative mining (HNM). In real scene of object detection, the imbalance between backgrounds and objects could be as extreme as 10<sup>4</sup> ~ 10<sup>5</sup> background windows to every object. HNM aims to deal with the problem of imbalanced data during training.
  - Bootstrap. Bootstrap in object detection refers to a group of training techniques in which the training starts with a small part of background samples and then iteratively add new misclassified backgrounds during the training process.
  - balance the weights between the positive and negative windows. However, this kind of method cannot completely solve the imbalanced data problem.
  - design new loss functions, put more focus on hard, misclassified examples.

### Acceleration of object detection
- Level 1: speed up of numerical computations (integral image, FFT, vector quantization, reduced rank approx)
- Level 2: speed up of detector engine (network pruning and quantification, lightweight network design)
- Level 3: speed up of detection pipeline (feature map shared computation, speed up of classifier, cascaded detection)

#### feature map shared computation
Compute the feature map of the whole image only once before sliding window on it, such as how HOG feature map comes. Another example coming from CNN, weight sharing. Scale computational redundancy, directly scale the features rather than the images.

#### speed up of classifiers
Linear classifiers are low computational complexity, while kernel-based SVM has higher accuracy. Use reduced Set Vectors, piece-wise linear approximation for decision boundary, sparse encoding to speed up traditional SVM.

#### cascaded detection
