# Roboflow Object Tracking Example

Object tracking using Roboflow Inference API and Zero-Shot (CLIP) Deep SORT. Read more in our
[Zero-Shot Object Tracking announcement post](https://blog.roboflow.com/zero-shot-object-tracking/).

![Example fish tracking](https://user-images.githubusercontent.com/870796/130703648-8af62801-d66c-41f5-80ae-889301ae9b44.gif)

Example object tracking courtesy of the [Roboflow Universe public Aquarium model and dataset](https://universe.roboflow.com/brad-dwyer/aquarium-combined). You can adapt this to your own dataset on Roboflow or any pre-trained model from [Roboflow Universe](https://universe.roboflow.com).

# Overview

Object tracking involves following individual objects of interest across frames. It
combines the output of an [object detection](https://blog.roboflow.com/object-detection) model
with a secondary algorithm to determine which detections are identifying "the same"
object over time.

Previously, this required training a special classification model to differentiate
the instances of each different class. In this repository, we have used
[OpenAI's CLIP zero-shot image classifier](https://blog.roboflow.com/clip-model-eli5-beginner-guide/)
to create a universal object tracking repository. All you need is a trained object
detection model and CLIP handles the instance identification for the object tracking
algorithm.

# Getting Started

Colab Tutorial Here:

<a href="https://colab.research.google.com/drive/1aU7Jq668oMlUx6bYVv3vAQbXVLpIllNH"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a>

## Training your model

To use the Roboflow Inference API as your detection engine:

Upload, annotate, and train your model on Roboflow with [Roboflow Train](https://docs.roboflow.com/train).
Your model will be hosted on an inference URL.

To use YOLOv5 as your detection engine:

Follow Roboflow's [Train YOLOv5 on Custom Data Tutorial](https://blog.roboflow.com/how-to-train-yolov5-on-a-custom-dataset/)

The YOLOv5 implementation uses [this colab notebook](https://colab.research.google.com/drive/1gDZ2xcTOgR39tGGs-EZ6i3RTs16wmzZQ)

The YOLOv5 implementation is currently compatible with this commit hash of YOLOv5 `886f1c03d839575afecb059accf74296fad395b6`

## Performing Object Tracking

###Clone repositories

```
git clone https://github.com/roboflow-ai/zero-shot-object-tracking
cd zero-shot-object-tracking
git clone https://github.com/openai/CLIP.git CLIP-repo
cp -r ./CLIP-repo/clip ./clip             // Unix based
robocopy CLIP-repo/clip clip\             // Windows
```

###Install requirements (python 3.7+)

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

###Install requirements (anaconda python 3.8)
```
conda install pytorch torchvision torchaudio -c pytorch
conda install ftfy regex tqdm requests pandas seaborn
pip install opencv pycocotools tensorflow
```

###Run with Roboflow

```bash

python clip_object_tracker.py --source data/video/fish.mp4 --url https://detect.roboflow.com/playing-cards-ow27d/1 --api_key ROBOFLOW_API_KEY --info
```

**NOTE you must provide a valid API key from [Roboflow](docs.roboflow.com)

###Run with YOLOv5
```bash

python clip_object_tracker.py --weights models/yolov5s.pt --source data/video/fish.mp4 --detection-engine yolov5 --info
```

###Run with YOLOv4
To use YOLOv4 for object detection you will need pretrained weights (.weights file), a model config for your weights (.cfg), and a class names file (.names). Test weights can be found here https://github.com/AlexeyAB/darknet. [yolov4.weights](https://github.com/AlexeyAB/darknet/releases/download/darknet_yolo_v3_optimal/yolov4.weights) [yolov4.cfg](https://raw.githubusercontent.com/AlexeyAB/darknet/master/cfg/yolov4.cfg)
```
python clip_object_tracker.py --weights yolov4.weights --cfg yolov4.cfg --names coco.names --source data/video/cars.mp4 --detection-engine yolov4 --info
```
(by default, output will be in runs/detect/exp[num])

<figure class="video_container">
  <video controls="true" allowfullscreen="true" poster="path/to/poster_image.png">
    <source src="data/demo/cards.mp4" type="video/mp4">
  </video>
</figure>

Help

```bash
python clip_object_tracker.py -h
```
```
--weights WEIGHTS [WEIGHTS ...]  model.pt path(s)
--source SOURCE                  source (video/image)
--img-size IMG_SIZE              inference size (pixels)
--confidence CONFIDENCE          object confidence threshold                      
--overlap OVERLAP                IOU threshold for NMS
--thickness THICKNESS            Thickness of the bounding box strokes
--device DEVICE                  cuda device, i.e. 0 or 0,1,2,3 or cpu
--view-img                       display results
--save-txt                       save results to *.txt
--save-conf                      save confidences in --save-txt labels
--classes CLASSES [CLASSES ...]  filter by class: --class 0, or --class 0 2 3
--agnostic-nms                   class-agnostic NMS
--augment                        augmented inference
--update                         update all models
--project PROJECT                save results to project/name
--name NAME                      save results to project/name
--exist-ok                       existing project/name ok, do not increment
--nms_max_overlap                Non-maxima suppression threshold: Maximum detection overlap.
--max_cosine_distance            Gating threshold for cosine distance metric (object appearance).
--nn_budget NN_BUDGET            Maximum size of the appearance descriptors allery. If None, no budget is enforced.
--api_key API_KEY                Roboflow API Key.
--url URL                        Roboflow Model URL.
--info                           Print debugging info.
--detection-engine               Which engine you want to use for object detection (yolov5, yolov4, roboflow).
```
## Acknowledgements

Huge thanks to:

- [yolov4-deepsort by theAIGuysCode](https://github.com/theAIGuysCode/yolov4-deepsort)
- [yolov5 by ultralytics](https://github.com/ultralytics/yolov5)
- [Deep SORT Repository by nwojke](https://github.com/nwojke/deep_sort)
- [OpenAI for being awesome](https://openai.com/blog/clip/)
