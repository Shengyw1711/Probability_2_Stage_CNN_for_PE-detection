# Probability_2_Stage_CNN_for for Pulmonary Embolism(PE) detection and segmentation

這是一支"基於機率的兩階段卷積神經網路於肺栓塞檢測"的程式,
使用的軟體/套件包括Python 3, Keras和>=1.10或<2.0的TensorFlow.程式的靈感來自於[Mask RCNN](https://github.com/matterport/Mask_RCNN). 
本研究修改了它的候選區域生成網路(RPN)以符合研究所需. 
這支程式對於肺栓塞這種醫學影像中的小物件檢測有良好的性能. 

程式內容包括:
* 卷積神經網路(CNN)的程式(使用R)
* 建立高斯混合模型（GMM,用於生成機率分佈）的程式碼和期望值最大化（EM）演算法的程式碼
* 候選區域生成網路(RPN)的程式碼

# 訓練資料來源
需要下載PE影像資料集 [here](https://figshare:com/authors/MojtabaMasoudi/5215238).

The dataset is provided in dicom form, you should change it to jpg form, first. After that,
you should generate a label file for the coco dataset using the mask label provided by the dataset, 
and replace the file in the [annotations](annotations) directory.
you can also use the label file provided by us. If you use our label files, 
You need to rename each image in the following format: "%2d%3d.jpg" % (patient_id, slice_index),
and divide the data set to 3 parts, including: train, test and val, with the image name contained in each label file. 

The dataset includes 35 patients. A total of 8,792 CTPA images with
the size of 512×512 pixels are included, of which 2,304
contained lesion areas with 3781 PE regions of interest(PE-ROIs) altogether.More than 85% of these PE-ROIs are small
objects with the square root of the area≤32 pixels which only
occupy on average 0.3% of the image area.

# Getting started
Install the required package:
```
pip install -r requirements.txt
```

* [sampling.sample.py](sampling/sample.py) is the module to build GMM and sample points from GMM and save it to the 
[locations](locations) directory. you can run it and plot the GMM in a 3D Coordinate System.
```
python sampling/sample.py
```

* [pulmonary_embolism.py](pulmonary_embolism.py) is the main file to train and validate Probability_2_Stage_CNN.
You can download pre-trained COCO weights (mask_rcnn_coco.h5) from the [releases page](https://github.com/matterport/Mask_RCNN/releases)
to init the weight of the model.You can also use the [model](model/mask_rcnn_pulmonary.h5) we have pre-train to evaluate Probability_2_Stage_CNN directly.

To train your own model, run:
```
python pulmonary_embolism.py training --dataset="your dataset path" --model="your model weight file.h5"
```
To evaluate the model, run:
```
python pulmonary_embolism.py inference --dataset="your dataset path" --model="your model weight file.h5"
```


 







