# ScanNet Benchmark Tasks

Train/val/test split for ScanNet is given in [Benchmark](Benchmark).

## Installation
Training tasks use [Torch7](http://torch.ch/docs/getting-started.html), with torch packages `cudnn`, `cunn`, `hdf5`, `xlua`.

Code to generate the training data from ScanNet data to come soon.

## Tasks

### 3D Object Classification

Classification of partially-scanned objects, voxelized to 30x30x30. We use the [3D CNN framework](https://github.com/charlesq34/3dcnn.torch) of [Qi et. al.](https://arxiv.org/abs/1604.03265) with an additional data channel encoding known/unknown voxels according to the camera scanning trajectory.

* Training:  
 run
 ```
 th train_partial.lua --train_data [path to train h5 file list] --test_data [path to test h5 file list]
 ```
 with the appropriate paths to the train/test data (use '--help' to see more options)
 
* Citation:  
```
@inproceedings{qi2016volumetric,
    title={Volumetric and Multi-View CNNs for Object Classification on 3D Data},
    author={Charles Ruizhongtai Qi and Hao Su and Matthias Nie{\ss}ner and Angela Dai and Mengyuan Yan and Leonidas Guibas},
    journal={Proc. Computer Vision and Pattern Recognition (CVPR), IEEE},
    year={2016}
}
```
 
### 3D Object Retrieval

Nearest neighbor 3d model retrieval based on the features of the 3D object classification model. The code in [ObjectRetrieval](ObjectRetrieval) uses the feature extraction and nearest neighbor code from [3dmodel_feature](https://github.com/charlesq34/3dmodel_feature).

To compute nearest neighbors, first extract the features from both the query and the database by: 
 ```
 th generate_feat.lua --model [model] --h5_list_path [path to h5 file list to generate features for] --partial_data [use this flag if partial data] --file_label_file [assocation from h5 data to names and labels] --output_file [output file (features)] --output_name_file [output file (names)] --classes_file [optional, defines set of classes to consider]
 ```
then compute the nearest neighbors with:
 ```
 python nearest_neighbor.py --query_feats [feature file from generate_feat] --database_feats [feature file from generate_feat] --database_models [names corresponding to database feats] --output [output file] 
 ```
