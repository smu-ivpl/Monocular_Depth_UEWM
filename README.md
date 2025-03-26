# Enhanced Monocular Depth Estimation Network Based on Uncertainty Edge Weighting Mask.
# Publication: Tsinghua Science and Technology (in press) (March 26, 2025)
#### Ye-Ji Kim and Byung-Gyu Kim
#### Intelligent Vision Processing Lab. (IVPL), Sookmyung Women's University, Seoul, Republic of Korea

---

# Summary of paper

#### Abstract
> Depth estimation acts as an important factor for understanding spatial relations in the field of computer vision. In particular, it plays a key role in detecting obstacles and planning safe routes in autonomous driving technology. To ensure stability, improving the accuracy of depth estimation is emerging as an important issue, and research direction in the field of computer vision is also developing with the aim of improving accuracy.
Based on the analysis of an outdoor dataset in road driving, we investigate that depth network is difficult to predict accurate depth in distant regions.
Accordingly, this thesis aims to improve the overall performance by improving depth prediction accuracy in far-distance areas. For this purpose, we devise new modules in the encoder and decoder, respectively.
In the process of feature extraction, we propose an encoder modeling that can obtain information about distant regions where has a large loss compared to the near region.
In order to obtain information about distant areas, we employ an auxiliary network. By combining the features that are aware of this information by each layer, the reinforced features are extracted for the information about the uncertainty area.
In addition, we propose a methodology for generating a weighted edge map from a distance to allow the decoder generating depth to learn the edge information of the distant region.
To verify this, the performance evaluation on the distant region is taken for showing the improvement of 0.006, 0.073, 0.879, and 0.303 for Abs Rel, Sq Rel, RMSE, and RMSE log, respectively, on the KITTI dataset. For the DDAD dataset, improvement of 0.417, 0.242, 2.678, and 0.333 is observed respectively.
From these results, the proposed scheme can improve the overall performance by emphasizing the feature of the far-distant region.


#### Network Architecture
![architecture figure](./images/architecture.jpg)


#### Experimental Results

* KITTI
  
| Model |  Abs Rel | Sq Rel |  RMSE | Checkpoint | 
| ------| -----| ------- | ------ | -------------|
| DepthFormer | 0.052| 0.156| 2.133| [[Link]](https://github.com/zhyever/Monocular-Depth-Estimation-Toolbox) |
| DepthFormer+UEWM | 0.050 | 0.144	| 2.105 | [[Google Drive]](https://drive.google.com/drive/folders/1P_Y5Plzu9KsA-8mrzR2n0sDq5w8xE4nl?usp=sharing)
| GEDepth | 0.048| 0.142 | 2.050 | [[Link]](https://github.com/qcraftai/gedepth/tree/main) |
| GEDepth+UEWM | 0.047 | 0.138	| 2.033 | [[Google Drive]](https://drive.google.com/drive/folders/1P_Y5Plzu9KsA-8mrzR2n0sDq5w8xE4nl?usp=sharing)
| MDEUncertainty | 0.052| 0.156 | 2.133 | [[Link]](https://github.com/XiangMochu/MDEUncertainty) |
| MDEUncertainty+UEWM | 0.049 | 0.142	| 2.097 | [[Google Drive]](https://drive.google.com/drive/folders/1P_Y5Plzu9KsA-8mrzR2n0sDq5w8xE4nl?usp=sharing)

* DDAD
  
| Model |  Abs Rel | Sq Rel |  RMSE | Checkpoint | 
| ------| -----| ------- | ------ | -------------|
| DepthFormer | 0.052| 0.156| 2.133| [[Link]](https://github.com/zhyever/Monocular-Depth-Estimation-Toolbox) |
| DepthFormer+UEWM | 0.049 | 0.144	| 2.061| [[Google Drive]](-)
| GEDepth | 0.052| 0.156| 2.133| [[Link]](https://github.com/qcraftai/gedepth/tree/main) |
| GEDepth+UEWM | 0.049 | 0.144	| 2.061| [[Google Drive]](-)

![experiments figure](./images/ExperimentalResults/cap_qual.JPG)

---

# Training

* DepthFormer+proposed mehtod run
<pre><code>cd depthformer_exp
bash tools/dist_train.sh configs/depthformer/depthformer_swinl_22k_w7_kitti.py 4    </code></pre>

* GEDepth+proposed mehtod run
<pre><code>cd gedepth_exp
bash tools/dist_train.sh configs/depthformer/depthformer_a.py 4    </code></pre>

* MDEUncertainty+proposed mehtod run
<pre><code>cd mdeuncertainty_exp
python train.py --dataset kitti --encoder swin --reg_mode lin_cls --reg_supervision regression_l1 --prob_supervision soft_label --uncert_supervision error_uncertainty_ranking    </code></pre>
  
---

# Testing

* DepthFormer+proposed mehtod run
<pre><code>cd depthformer_exp
python tools/test.py configs/depthformer/depthformer_swinl_22k_w7_kitti.py ${CHECKPOINT_FILE}    </code></pre>

* GEDepth+proposed mehtod run
<pre><code>cd gedepth_exp
bash tools/dist_test.sh  configs/depthformer/depthformer_a.py ${CHECKPOINT_FILE} 1    </code></pre>

* MDEUncertainty+proposed mehtod run
<pre><code>cd mdeuncertainty_exp
python eval.py ${CHECKPOINT_FILE} </code></pre>
  
CHECKPOINT_FILE means pretrained weight file.
  
---

# Acknowledgements

The codes are heavily based on DepthFormer, GEDepth, and MDEUncertainty. I appreciate their outstanding work.
* https://github.com/zhyever/Monocular-Depth-Estimation-Toolbox
* https://github.com/qcraftai/gedepth/tree/main
* https://github.com/XiangMochu/MDEUncertainty

