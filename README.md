# Download LiDARHuman26M dataset
- Download the dataset from the link: http://www.lidarhumanmotion.net/lidarcap/.
- Download the weight file from the link: https://pan.baidu.com/s/1J0WEdkVCE4vlfBb1RWZl9w  code: oafj
- Download `basicModel_neutral_lbs_10_207_0_v1.0.0.pkl`,`J_regressor_extra.npy` from link: https://smpl.is.tue.mpg.de/
and put it in data directory.


If you want to deal with your own dataset, refer to `datasets/preprocess/lidarcap.py` to process your data into a format can be handled by LiDARCap.

- You could enter the directory `datasets/preprocess`, and then run the commands below:
  
  ```shell
  python lidarcap.py dump --seqlen 1 --name lidarcap_test --ids 7,24,29,41 # generate lidarcap_test.hdf5
  python lidarcap.py dump --seqlen 1 --name lidarcap_train --ids 5,6,8,25,26,27,28,30,31,32,33,34,35,36,37,38,39,40,42 # generate lidarcap_train.hdf5
  ```

- You could also download the processed `hdf5` files from [Baidu Drive](https://pan.baidu.com/s/1r85f-tiPJf48Ut_jrLPXNw?pwd=1234)

# TRAIN or EVAL
### 1. Modify the info

- Modify `base.yaml`to set `DATASET_DIR` to the path where your dataset is located.

- Update the relevant information for `wandb` in `tools/common.py`.

### 2. Build Environment

```
conda create -n lidar_human python=3.7
pip install torch==1.8.1+cu111 torchvision==0.9.1+cu111 torchaudio==0.8.1 -f https://download.pytorch.org/whl/torch_stable.html
pip install "git+git://github.com/erikwijmans/Pointnet2_PyTorch.git#egg=pointnet2_ops&subdirectory=pointnet2_ops_lib"（或者下载github然后pip install pointnet2_ops_lib/.）
pip install -r requirements.txt
```

### 3. Train 
```
python train.py --threads x --gpu x --dataset lidarcap
```

### 4. Eval
```
python train.py --threads x --gpu x --dataset lidarcap --ckpt_path best-train-loss21.pth --eval --eval_bs 4 --debug
```

```
# Output reference
LiDARCap >> 01/14 13:02:28 >> Launching on GPUs 5
100%|███| 376/376 [00:13<00:00, 27.09it/s]
EVAL LOSS 0.03766141264907461
100%|██████████| 1502/1502 [00:01<00:00, 972.60it/s] 
poses_to_vertices: 100%|██████████| 188/188 [00:04<00:00, 40.94it/s]
poses_to_vertices: 100%|██████████| 188/188 [00:04<00:00, 46.83it/s]
poses_to_joints: 100%|██████████| 188/188 [00:03<00:00, 57.27it/s]
poses_to_joints: 100%|██████████| 188/188 [00:03<00:00, 57.23it/s]
44.9119471013546
79.38986271619797
66.94589555263519
102.49917954206467
0.8611590795605859
0.9491181896360409
```

### Citation
```
@article{Li2022LiDARCapLM,
  title={LiDARCap: Long-range Markerless 3D Human Motion Capture with LiDAR Point Clouds},
  author={Jialian Li and Jingyi Zhang and Zhiyong Wang and Siqi Shen and Chenglu Wen and Yuexin Ma and Lan Xu and Jingyi Yu and Cheng Wang},
  journal={2022 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
  year={2022},
  pages={20470-20480},
  url={https://api.semanticscholar.org/CorpusID:247762999}
}```