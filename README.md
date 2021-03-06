# SSL-VQA
This repository contains code modified from [here](https://github.com/jialinwu17/self_critical_vqa), many thanks!

## Requirements
* python 3.6.8

* pytorch 1.0.1 

* zarr

* tdqm

* spacy

* h5py

## Download and preprocess the data

```
cd data 
bash download.sh
python preprocess_image.py --data trainval
python create_dictionary.py --dataroot vqacp2/
python preprocess_text.py --dataroot vqacp2/ --version v2
cd ..
```

## Training
* Train our model with multi-label VQA loss
```
CUDA_VISIBLE_DEVICES=0 python main.py --dataroot data/vqacp2/ 
--img_root data/coco/ --output saved_models_cp2/ --self_loss_weight 3 --ml_loss
```
* Train our model with corss-entropy VQA loss
```
CUDA_VISIBLE_DEVICES=0 python main.py --dataroot data/vqacp2/ 
--img_root data/coco/ --output saved_models_cp2/ --self_loss_weight 1.2 --ce_loss
```
* Train the model with 80% of the original training set
```
CUDA_VISIBLE_DEVICES=0 python main.py --dataroot data/vqacp2/ 
--img_root data/coco/ --output saved_models_cp2/ --self_loss_weight 3 --ml_loss --ratio 0.8
```

## Evaluation
* A json file of results from the test set can be produced with:
```
CUDA_VISIBLE_DEVICES=0 python test.py --dataroot data/vqacp2/ --img_root data/coco/ --checkpoint_path saved_models_cp2/best_model.pth --output saved_models_cp2/result/
```
* Compute detailed accuracy for each answer type:
```
python comput_score.py --input saved_models_cp2/result/XX.json --dataroot data/vqacp2/
```




