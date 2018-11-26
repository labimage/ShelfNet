# ShelfNet 
* [Link to my homepage](https://juntangzhuang.com)
* This is repository for my paper [ShelfNet for real-time semantic segmentation](https://juntang-zhuang.github.io/files/ShelfNet_2019.pdf), and avhieves both faster inference speed and higher segmentation accuracy, compared with other real-time models such as Lightweight-RefineNet.
* This repository only deals with Pascal dataset. Code for experiments on Cityscapes dataset can be found [here](https://github.com/juntang-zhuang/ShelfNet-Cityscapes).
* This implementation is based on [torch-encoding](https://github.com/zhanghang1989/PyTorch-Encoding). Main difference is the structure of the model. </br></br>
**Results**</br>
* We tested ShelfNet with ResNet50 and ResNet101 as the backbone respectively: they achieved **59 FPS** and **42 FPS** respectively on a GTX 1080Ti GPU with a 512x512 input image. 
* On PASCAL VOC 2012 test set, it achieved **84.2%** mIoU with ResNet101 backbone and **82.8%** mIoU with ResNet50 backbone.
* It achieved **75.8%** mIoU with ResNet50 backbone on Cityscapes dataset.


# Requirements
* Please refer to [torch-encoding](https://github.com/zhanghang1989/PyTorch-Encoding) for implementation on synchronized batch-normalization layer.
* PyTorch 0.4.1
* requests
* nose
* scipy
* tqdm
* Other requirements by [torch-encoding](https://github.com/zhanghang1989/PyTorch-Encoding).

# How to run
**Environment setup and data preparation**
* run ```python setup.py install``` to install torch-encoding
* make sure you have the same path for a datset in ```/scripts/prepare_xx.py``` and ```/encoding/datasets/xxx.py```, default path is ```~/.encoding/data```, which is a hidden folder. You will need to type ```Ctrl + h``` to show is in ```Files```
* run ```cd scripts```
* run ```python prepared_xx.py ``` to prepare datasets, including MS COCO, PASCAL VOC, PASCAL Aug, PASCAL Context </br></br>

**Configurations** (refer to /experiments/option.py)</br>
* --model: which model to use, default is ```shelfnet```, other options include ```pspnet```, ```encnet```,```fcn```
* --backbone: backbone of the model, ```resnet50``` or ```resnet101```
* --dataset: which dataset to train on, ```coco``` for MS COCO, ```pascal_aug``` for augmented PASCAL,```pascal_voc``` for PASCAL VOC,```pcontext``` for pascal context.
* --aux: if type ```--aux```, the model will use auxilliray layer, which is a FCN head based on the final block of backbone.
* --se_loss: a context module based on final block of backbone, the shape is 1xm where m is number of categories. It penalizes whether a category is present or not.
* --resume: default is None. It specifies the checkpoint to load
* --ft: fine tune flag. If set as True, the code will resume from checkpoint but forget previous best accuracy and optimizer information.
* --checkname: folder name to store trained weights
* Other parameters are trevial, please refer to /experiments/segmentation/option.py for more details

**Training scripts**
* run ```cd /experiments/segmentation```
* pre-train ShelfNet50 on COCO, ```python train.py --backbone resnet50 --dataset coco --aux --se_loss --checkname ShelfNet50_aux```
* fine-tune ShelfNet50 on PASCAL_aug, ```python train.py --backbone resnet50 --dataset pascal_aug --aux --se_loss --checkname ShelfNet50_aux --resume /runs/coco/ShelfNet50_aux_se/model_best.pth.tar -ft```
* fine-tune ShelfNet50 on PASCAL VOC, ```python train.py --backbone resnet50 --dataset pascal_voc --aux --se_loss --checkname ShelfNet50_aux --resume /runs/pascal_aug/ShelfNet50_aux_se/model_best.pth.tar -ft```

**Test scripts**
* test on PASCAL_VOC, ```python test.py --backbone resnet50 --dataset pascal_voc --aux --se_loss --resume /runs/pascal_voc/ShelfNet50_aux_se/model_best.pth.tar```
* Similar experiments can be performed on ShelfNet101

# Examples on Pascal VOC datasets
![Pascal results](https://github.com/juntang-zhuang/ShelfNet/blob/master/video_demo/Pascal_results.png) </br>

# Video Demo on Cityscapes datasets
**Video demo of ShelfNet50 on Cityscapes**
<a href="url"><img src="https://github.com/juntang-zhuang/ShelfNet/blob/master/video_demo/shelfnet50_demo.gif" align="left"  width="1000" ></a> </br>
**Video demo of ShelfNet101 on Cityscapes** </br>
<a href="url"><img src="https://github.com/juntang-zhuang/ShelfNet/blob/master/video_demo/shelfnet101_demo.gif" align="left"  width="1000" ></a> </br>
