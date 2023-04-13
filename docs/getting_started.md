## Installation
Follow https://github.com/fundamentalvision/BEVFormer/blob/master/docs/install.md to prepare the environment.

VJ: I ran into a dependency conflicts when I followed the BEVFormer guideline. My solution is to specify version for
some libs. Specifically,
1. ....
2. mmcv-full==1.4.0
3. pip install opencv-python==3.4.17.63
4. ....
5. mmsegmentation==0.14.1
6. 
``` 
pip install scikit-image==0.17.2
pip install numpy==1.21.3
pip install matplotlib==3.5.2
pip install pandas==1.2.5
```
7. ....

Note: .... means follow the official steps.

## Preparing Dataset
1. Download the gts and annotations.json we provided. You can download our imgs.tar.gz or using the original sample files of the nuScenes dataset.
And rename the `images` folder to `samples`.

2. Download the CAN bus (v1.0) expansion data and maps [HERE](https://www.nuscenes.org/download).

3. Download v1.0-trainval_meta.tgz from [HERE](https://www.nuscenes.org/download) -- Full dataset (v1.0) -- Trainval -- Metadata. 
And put the uncompressed folders under `./data/occ3d-nus`.

4. Organize your folder structure as below：
```
Occupancy3D
├── projects/
├── tools/
├── ckpts/
│   ├── r101_dcn_fcos3d_pretrain.pth
├── data/
│   ├── can_bus/
│   ├── occ3d-nus/
│   │   ├── maps/
│   │   ├── samples/     # You can download our imgs.tar.gz or using the original sample files of the nuScenes dataset
│   │   ├── v1.0-trainval/
│   │   ├── gts/
│   │   │── annotations.json
```


4. Generate the info files for training and validation:
```
python tools/create_data.py occ --root-path ./data/occ3d-nus --out-dir ./data/occ3d-nus --extra-tag occ --version v1.0-trainval --canbus ./data --occ-path ./data/occ3d-nus
``` 

## Training
before training, two libs are necessary:
```angular2html
pip install protobuf
pip install tensorboard
```

```
./tools/dist_train.sh projects/configs/bevformer/bevformer_base_occ.py 8
```

## Testing
```
./tools/dist_test.sh projects/configs/bevformer/bevformer_base_occ.py work_dirs/bevformer_base_occ/epoch_24.pth 8
```
You can evaluate the F-score at the same time by adding `--eval_fscore`.


### Performance

model name|weight| mIoU | others | barrier | bicycle | bus | car | construction_vehicle | motorcycle | pedestrian | traffic_cone | trailer |  truck | driveable_surface | other_flat | sidewalk | terrain | manmade | vegetation | 
----|:----------:| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :----------------------: | :---: | :------: | :------: |
bevformer_base_occ|[Google Drive](https://drive.google.com/file/d/1NyoiosafAmne1qiABeNOPXR-P-y0i7_I/view?usp=share_link)| 23.67 | 5.03 | 38.79 | 9.98 | 34.41 | 41.09 | 13.24 | 16.50 | 18.15 | 17.83 | 18.66 | 27.7 | 48.95 | 27.73 | 29.08 | 25.38 | 15.41 | 14.46 | 


