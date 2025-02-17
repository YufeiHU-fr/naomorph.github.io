# Deep Morphological Networks with Neural Architecture Search
This is a project to study the effect of morphological network on image classification and edge detection based on Neural Architecture Optimization.

There are two sub-projects, one is for image classification which is NAO-V2 and the other is for edge detection which is NAO_Seg_v2:

1. NAO-V2: Official implementation of NAO-V2: [Understanding and Improving One-shot Neural Architecture Optimization]. Thanks to [Neural Architecture Optimization](https://arxiv.org/abs/1808.07233), we proposed a new morphological network which has better performance on CIFAR10 and CIFAR100 than traditional convolutional layers.

2. Released with the paper [Richer Convolutional Features for Edge Detection](https://arxiv.org/abs/1612.02103). The purpose of this project is for edge detection. On the basis of ResNet-101, a decoder is added(where we call the new DNN **NAO-Multi-scale**) in order to get better edge detection effect and prove the superiority of the morphological network. The decoder is based on cells which are generated by NAO-V2.


## Citations

If you find our work useful in your research, please consider citing:

    @misc{hu2021learning,
      title={Learning Deep Morphological Networks with Neural Architecture Search}, 
      author={Yufei Hu and Nacim Belkhir and Jesus Angulo and Angela Yao and Gianni Franchi},
      year={2021},
      eprint={2106.07714},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
}
    
## Requirements
* Pytorch >= 1.0.0
* Dataset for edge detection(provided by [Richer Convolutional Features for Edge Detection repo](https://github.com/yun-liu/rcf))
  * [http://mftp.mmcheng.net/liuyun/rcf/data/bsds_pascal_train_pair.lst]
  * [http://mftp.mmcheng.net/liuyun/rcf/data/HED-BSDS.tar.gz]
  * [http://mftp.mmcheng.net/liuyun/rcf/data/PASCAL.tar.gz]
* Eval method provided by [hed](https://github.com/xwjabc/hed/tree/c8ed5abc4d2b6ad2862b0d61cf6184ce2cdf3cae/eval), but please replace the file ```eval_edge.m``` where we just modified the path and we keep the same for other parameters for fairness.
* and other requirements... (cv2, numpy , etc.)

## Usage for classification
* To search the CNN architecture with the morphological network, please refer firstly to:
```
bash ./NAO_V2/train_search_cifar10.sh
```
* After model searching, we need to train CIFAR 10, then refer to:

| Dataset       | Script        | Time          | Top1 Error Rate  |
| ------------- | ------------- | ------------- | ------------- | 
|CIFAR-10       | ./NAO_V2/train_NAONet_V2_36_cifar10.sh | 2 days | **2.63%** | 
```
bash ./NAO_V2/train_NAONet_V2_36_cifar10.sh
```

## Usage for edge detection
### BSD500
* Firstly download the dataset and then use NAO to search the CNN architecture with the morphological gradient network, refer to:
```
bash ./NAO_Seg_v2/train_search_BSD500.sh
```
* After model searching, we need to train BSD500, then refer to:

| Dataset       | Script        | Time          | ODS  | OIS  |AP  |R50  |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | 
|BSD500      | ./NAO_Seg_v2/train_NAOUNet_Berkeley.sh | 2 days | **0.814±0.001** |  **0.831±0.001** | **0.850±0.002** | **0.908±0.005** |
```
bash ./NAO_Seg_v2/train_NAOUNet_Berkeley.sh
```
* After training, we can get test predicted features maps by runing:
```
bash ./NAO_Seg_v2/test_NAOUNet_Berkeley.sh
```
### Evaluation for edge detection

We are the first to add ODS python code to filter great architecture and save the better model during the process of training which helps to greatly reduce the time of evaluation. This is because we don't need to do the evaluation which cost about 2hours for each checkpoint. We just need to evaluate the predicted features maps of the best and last checkpoint.

* Firstly, we need to modify the path to the correct address where you can refer to [eval_edge.m](https://github.com/giannifranchi/NAO_morpho/blob/master/NAO_Seg_v2/eval/eval_edge.m)
* Then download the complete evaluation project based on matlab provided by [HED](https://github.com/xwjabc/hed/tree/master/eval), and runing:
```
(echo "data_dir = '../output/epoch-x-test'"; cat eval_edge.m)|matlab -nodisplay -nodesktop -nosplash
```
## URL
* [Pretrained model for edge detection](https://drive.google.com/file/d/17OZhn87HmaoaJD1oYkE3MDaBCySnaV0i/view?usp=sharing)
* [Test features maps](https://drive.google.com/file/d/1CD9aqNStdDCIcmz-bYDvPs-N9fM8tV5p/view?usp=sharing)
* [Arch examples](https://drive.google.com/file/d/1Y7edbOdHLPlnDsiO1Bm2RZGB2jAYwl4H/view?usp=sharing)

## License
The codes and models in this repo are released under the GNU GPLv3 license.

## Acknowledgment
Thanks to the contributors of NAO, RCF, HED.
```
@misc{luo2019neural,
      title={Neural Architecture Optimization}, 
      author={Renqian Luo and Fei Tian and Tao Qin and Enhong Chen and Tie-Yan Liu},
      year={2019},
      eprint={1808.07233},
      archivePrefix={arXiv},
      primaryClass={cs.LG}
}
```
```
@article{Liu_2019,
   title={Richer Convolutional Features for Edge Detection},
   volume={41},
   ISSN={1939-3539},
   url={http://dx.doi.org/10.1109/TPAMI.2018.2878849},
   DOI={10.1109/tpami.2018.2878849},
   number={8},
   journal={IEEE Transactions on Pattern Analysis and Machine Intelligence},
   publisher={Institute of Electrical and Electronics Engineers (IEEE)},
   author={Liu, Yun and Cheng, Ming-Ming and Hu, Xiaowei and Bian, Jia-Wang and Zhang, Le and Bai, Xiang and Tang, Jinhui},
   year={2019},
   month={Aug},
   pages={1939–1946}
}

```
```
@inproceedings{xie2015holistically,
  title={Holistically-nested edge detection},
  author={Xie, Saining and Tu, Zhuowen},
  booktitle={Proceedings of the IEEE International Conference on Computer Vision},
  pages={1395--1403},
  year={2015}
}
```
_This is not an official Microsoft product._
