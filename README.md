# yolo-optimizer-comparison

<img alt="license" src="https://img.shields.io/github/license/mashape/apistatus.svg"/>

This repository presents the results of a study on a comparative analysis of the choice of optimizers and their impact on the different phases of the deep learning life cycle, using YOLO-based object detection models, specifically YOLOv8n and YOLO12n for unmanned surface vehicles (USVs).

Five optimizers were evaluated, including SGD, Adam, NAdam, RAdam, and Adadelta, on various devices such as Tesla V100, A30, RTX 3090, and Jetson AGX Orin, comparing their impact on performance according to traditional metrics (mAP, recall, etc.), computational performance (FPS), and inference performance from an energy perspective.

A total operational efficiency score was used to identify the optimizer with the best balance between accuracy and energy consumption, which resulted in Adadelta consistently achieving the best balance for both YOLOv8 and YOLO12, given the challenges of the maritime/aquatic environment, which requires high accuracy, stability, and real-time energy efficiency for devices with limited resources.


For this study we have followed these steps:

## 1. Installation

### Python dependencies

The best way to install python dependencies is by using a virtual environment, to do so:

```bash
$ sudo apt install virtualenv
$ virtualenv -p python3 venv
$ source venv/bin/activate
$ pip install numpy
```
### Requirements 

To run the code, you will need to install the following dependencies beforehand:

* Ultralytics >=8.3.20
* Python
* Pytorch 
* Clearml 1.18.0 

To deactivate virtualenv, do by:

```bash
$ deactivate
```

## 2. Training

### Dataset structure 

To train the model, a format, in this case YOLOv8, must be followed and the training data set must be divided into three subdirectories.  
The data used to train the custom model in the USV context can be found here: [Datasense@CRAS](https://rdm.inesctec.pt/lv/dataset/nis-2022-001).  
An example of the directory structure is shown below:

```
Datasense@CRAS/ │
              ├── train/ │
              │   ├── images/
              │   ├── image1.jpg
              │   ├── image2.jpg
              │   ├── .... (other image files)  
              │   └── labels/
              │   ├── label1.txt
              │   ├── label2.txt
              │   ├── .... (other label files)     
              ├── valid/ │
              │   ├── image1.jpg
              │   ├── image2.jpg
              │   ├── .... (other image files)
              │   └── labels/
              │   ├── label1.txt
              │   ├── label2.txt
              │   ├── .... (other label files) 
              ├── test/ │
              │   ├── image1.jpg
              │   ├── image2.jpg
              │   ├── .... (other image files)
              │   └── labels/
              │   ├── label1.txt
              │   ├── label2.txt
              │   ├── .... (other label files) 
              └── data.yaml 
```

### CLI to Train the model
To run or launch the training you will need:

```bash
$ yolo detect train data=/path/data.yaml model=yolov8n.pt epochs=150 imgsz=640 plots=True save=True 
```
### Some examples of trained model predictions

<table>
  <tr>
    <td><img src="predict/detec1.png" alt="prediction 1" width="100%"></td>
    <td><img src="predict/detec2.png" alt="prediction 2" width="100%"></td>
  </tr>
  <tr>
    <td><img src="predict/detec3.png" alt="prediction 3" width="100%"></td>
    <td><img src="predict/detec4.png" alt="prediction 4" width="100%"></td>
  </tr>
</table>

## 3. Evaluation
### Performance metrics of each optimizer algorithms based on YOLOv8n

<img src="table/Alloptimizer.png" alt="bench 1" width="100%">

### Comparison between different optimizer algorithms YOLOv8n and YOLO12n models

<img src="table/twogpu.png" alt="bench 1" width="100%">

### GPU comparison using early stopping: Statistical Analysis

#### Summary of statistical results: main effects and relative efficiency

<img src="table/summary.png" alt="bench 1" width="100%">

#### Descriptive statistics by GPU and optimizer (mean ± standard deviation, n=12)  

<img src="table/summarydescriptive.png" alt="bench 1" width="100%">

#### Statistical analysis: comparison of the 120 training records on GPUs by optimizer

<img src="table/analysis.png" alt="bench 1" width="100%">

### Computing Continuum Analysis: From Cloud to Edge Deployment.

#### Inference Performance Evaluation
All measurements were obtained using TensorRT-optimized models in FP32 precision. Composite scores were calculated using:

$$
\mathrm{Score} = 100 - \left(
    W \cdot \frac{E}{N}
    + (1 - W) \cdot (100 - A)
\right)
$$

with \(W = 0.55\) and normalization constant \(N = 70.43\) W (maximum observed power consumption). Power measurements represent mean \(\pm\) standard deviation across test set inference.

<img src="table/efficiency_Nvidia.png" alt="bench 1" width="100%">

## 4. Publications

In process "A Comparative Study of Optimizer Algorithms for YOLO-Based Object Detection in USVs: Statistical Analysis and Mathematical Framework"

## 5. Acknowledgements

J.L.M. thanks the National Secretariat of Science, Technology and Innovation (SENACYT) of Panama for financial support during the completion of his PhD.

## 6. Funding

This paper has been partially funded by the EU (FEDER), the Spanish MINECO under grants PID2021-126576NB-I00 and PID2024-158311NB-I00 funded by MCIN/AEI/10.13039/501100011033 and by European Union ``ERDF A way of making Europe'' and the NextGenerationEU/PRT.