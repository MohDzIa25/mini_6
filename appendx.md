# Data And Preprocessing 
In this part, the way to download and preprocess data is demonstrated.
### Download FHM dataset published by Facebook
The distribution of FHM dataset in phase 1(train,dev_seen,test_seen) is shown below. Seen is just a kind of name and do not means that images in dev or test set apear in train set. You can download that dataset from this website: <https://hatefulmemeschallenge.com/#download>. And you can get more detailed information from its official paper:"The Hateful Memes Challenge:Detecting Hate Speech in Multimodal Memes"<https://arxiv.org/pdf/2005.04790.pdf>.

| DataSplit | Num-Hate | Num-Non-Hate | Total |
| ------ | ------ | ------ | ------|
| train | 3050 | 5450 | 8500 |
| dev_seen | 250 | 250 | 500 |
| test_seen | 500 | 500 | 1000 |
---
### Download OFF dataset
The table below illustrates the distribution of OFF dataset. You can download data from <https://drive.google.com/drive/folders/1hKLOtpVmF45IoBmJPwojgq6XraLtHmV6?usp=sharing> or seek for more information from <https://github.com/bharathichezhiyan/Multimodal-Meme-Classification-Identifying-Offensive-Content-in-Image-and-Text> and its official paper:"Multimodal Meme Dataset (MultiOFF) for Identifying Offensive Content in
Image and Text"<https://www.aclweb.org/anthology/2020.trac-1.6.pdf>.
| DataSplit | Num-Hate | Num-Non-Hate | Total |
| ------ | ------ | ------ | ------|
| train | 187 | 258 | 445 |
| dev_seen | 58 | 91 | 149 |
| test_seen | 58 | 91 | 149 |
---
### Data Preprocessing
Before preprocessing, because there are different kinds of image formats in OFF dataset, you'd better change their forms to .png. Besides, to avoid checking and changing codes, you should rename images in OFF dataset using numbers like 0001.png, 0002.png....If you want to apply our model to your own dataset, you also should change form and rename like that way. After that, you can preprocess data following data_utils part in <https://github.com/HimariO/HatefulMemesChallenge>. 
Make sure that: 
1. You can use docker in your server or it will be extremely difficult.
2. The server is not RTX3090Ti. Codes is not supported by 3090Ti.
3. You need to use google cloud vision API to detect entities. It costs us 600 dollars to detect about 20w images but luckily each account can get 300 dollars freely.

### Citation
```bibtex
@inproceedings{suryawanshi-etal-2020-MultiOFF,
title = "Multimodal Meme Dataset (MultiOFF) for Identifying Offensive Content in Image and Text",
author = "Suryawanshi, Shardul and Chakravarthi, Bharathi Raja and Arcan, Mihael and Buitelaar, Paul,
booktitle = "Proceedings of the Second Workshop on Trolling, Aggression and Cyberbullying ({TRAC}-2020)",
month = May,
year = "2020",
publisher = "Association for Computational Linguistics",}
```



# Official baseline recording and top_rank reproduction
In this part, we show three main things:
1. Recording results and codes of official baseline of FHM and OFF dataset. 
2. Reproduction results and codes of official baseline of OFF using AUROC metric.
3. Reproduction results and codes  of top_rank method of FHM dataset.
4. Experiment results that applying top_rank method to OFF dataset.

## results and codes of official baseline of FHM and OFF dataset
### FHM
You can reproduce official baseline following this link:<https://github.com/facebookresearch/mmf/tree/master/projects/hateful_memes> and some detailed information:<https://arxiv.org/pdf/2005.04790.pdf>. The baseline results are shown below:
| Model                          | Validation set | Test set |
|:-------------------------------|:--------------:|:-------------:|
|Image-Grid                      |Acc:52.73 AUROC:58.79   |Acc:52.00±1.04 AUROC:52.63±0.20     |
|Image-Region                    |Acc52.66 AUROC:57.98    |Acc52.13±0.40 AUROC:55.92±1.18     |
|Text BERT                       |Acc:58.26 AUROC:64.65   |Acc59.20±1.00 AUROC:65.08±0.87     |
|Late Fusion                     |Acc:61.53 AUROC:65.97   |Acc:59.66±0.64 AUROC:64.75±0.96|
|Concat BERT                     |Acc:58.60 AUROC:65.25   |Acc:59.13±0.78 AUROC:65.79±1.09|
|MMBT-Grid                       |Acc:58.20 AUROC:68.57   |Acc:60.06±0.97 AUROC:67.92±0.87      |
|MMBT-Region                     |Acc:58.73 AUROC:71.03   |Acc:60.23±0.87 AUROC:70.73±0.66      |
|ViLBERT                         |Acc:62.20 AUROC:71.13   |Acc:62.30±0.46 AUROC:70.45±1.16   |
|Visual BERT                     |Acc:62.10 AUROC:70.60   |Acc:63.20±1.06 AUROC:71.33±1.10    |
|ViLBERT CC                      |Acc:61.40 AUROC:70.07   |Acc:61.10±1.56 AUROC:70.03±1.77 |
|Visual BERT COCO                |Acc:65.06 AUROC:73.97   |Acc:64.73±0.50 AUROC71.41±0.46  |


### OFF
The result is shown below and you can reproduce following this link:<https://github.com/bharathichezhiyan/Multimodal-Meme-Classification-Identifying-Offensive-Content-in-Image-and-Text>. There is one thing that should attach your attention is that these result are based on F1_hate, precision_hate and recall_hate.
| Model                          | Precision_hate | Recall_hate | F1_hate |
|:-------------------------------|:--------------:|:-------------:|:-------------:|
|Text LR                          |0.58 |0.40| 0.48 |
|NB                               |0.52 |0.45 |0.49 |
|DNN                              |0.47 |0.54 |0.50|
|Stacked LSTM                     |0.39| 0.42 |0.40|
|BiLSTM                           |0.42 |0.23 |0.30|
|CNN                              |0.39| 0.84 |0.54|
|Image VGG16                      |0.41 |0.16 |0.24|
|Multi Stacked LSTM + VGG16       |0.40 |0.66| 0.50|
|BiLSTM + VGG16                   |0.40 |0.44| 0.41|
|CNNText + VGG16                  |0.38 |0.67 |0.48|


## Reproduction results and codes of official baseline of OFF using AUROC metric.
Results using AUROC are shown below and you can run codes in OFF_baseline documentory to reproduce the results. Before running codes, you should download environment files in this link<https://github.com/bharathichezhiyan/Multimodal-Meme-Classification-Identifying-Offensive-Content-in-Image-and-Text>
| Model                          | Precision_weighted | Recall_weighted | F1_weighted |
|:-------------------------------|:--------------:|:-------------:|:-------------:|
|BERT (unimodal)                  |56.4|56.1|57.7|
|StackedLSTM+VGG16                |46.3|37.3|61.1 |
|BiLSTM+VGG16                     |48.0|48.6|58.4|
|CNNText+VGG16                    |46.3|37.3|61.1|
|ERNIE-VIL                        |53.1|54.3|63.7|
|UNITER                           |58.1|57.8|58.4|
|VILLNA                           |57.3|57.1|57.6|
|VL-BERT                          |58.9|59.5|58.5|


## Reproduction results and codes of top_rank method of FHM dataset.
You can reproduce results of top_rank method of FHM dataset following this link:<https://github.com/HimariO/HatefulMemesChallenge>. Before you rerun codes, you should change its all config files. Because it treat both dev and train set as train set by default to get better scores in test set. That is, you should change dev+train to train. Besides, Visual_bert series need 4X14G, uniter series need 1X14G and ERNIE series need 1X12G. Make sure that you have enough GPU memory.

## Experiment results that applying top_rank method to OFF dataset.
First, you should follow the preprocessing part. Then the only change we do on top_rank method is that we changes its training steps, validation steps and warm up steps dividing ten(due to its scale of dataset relative to FHM dataset).
 


 ## Citation
 ```bibtex
@misc{singh2020mmf,
  author =       {Singh, Amanpreet and Goswami, Vedanuj and Natarajan, Vivek and Jiang, Yu and Chen, Xinlei and Shah, Meet and
                 Rohrbach, Marcus and Batra, Dhruv and Parikh, Devi},
  title =        {MMF: A multimodal framework for vision and language research},
  howpublished = {\url{https://github.com/facebookresearch/mmf}},
  year =         {2020}
}
```

 ```bibtex
@inproceedings{suryawanshi-etal-2020-MultiOFF,
title = "Multimodal Meme Dataset (MultiOFF) for Identifying Offensive Content in Image and Text",
author = "Suryawanshi, Shardul and Chakravarthi, Bharathi Raja and Arcan, Mihael and Buitelaar, Paul,
booktitle = "Proceedings of the Second Workshop on Trolling, Aggression and Cyberbullying ({TRAC}-2020)",
month = May,
year = "2020",
publisher = "Association for Computational Linguistics",}
```
