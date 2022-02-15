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
1.需要下載[PE影像資料集](https://figshare:com/authors/MojtabaMasoudi/5215238).  
2.影像資料集是以dicom檔儲存，需要先將其轉換為jpg格式。  
3.接著需要使用該數據集提供的Ground Truth標籤對下載下來的資料集建立一個標籤文件來替換[annotations](annotations)目錄中的文件.  

該影像資料集包括35名患者，總共納入 了8792張肺動脈電腦斷層血管攝影(CTPA)影像，大小為512×512像素，其中2304張包含病灶區域，總共有3781個肺栓塞(PE)的感興趣區域（PE-ROIs）。

# 執行程式
安裝所需的套件:
```
pip install -r requirements.txt
```

* [sampling.sample.py](sampling/sample.py) 是建立高斯混合模型(GMM)和採樣點並將其保存到 
[locations](locations)目錄的程式碼.執行後會在3D坐標系中繪製高斯混合模型(GMM).
```
python sampling/sample.py
```

* [pulmonary_embolism.py](pulmonary_embolism.py) 是訓練和驗證整個兩階段卷積神經網路的主要程式碼.
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


 







