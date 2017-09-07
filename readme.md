# 3D surface normal estimation from a single color image

This is a [torch](https://github.com/torch) implementation of FCN with short-cut link and a forced same sampling mask for up/down sampling. It has been demonstrated to work well for tasks that require estimating per-pixel values. Please go to http://pbrs.cs.princeton.edu and the following paper for more information. If you use this code, please cite:

	@article{zhang2016physically,
	  title={Physically-Based Rendering for Indoor Scene Understanding Using Convolutional Neural Networks},
	  author={Zhang, Yinda and Song, Shuran and Yumer, Ersin and Savva, Manolis and Lee, Joon-Young and Jin, Hailin and Funkhouser, Thomas},
	  journal={The IEEE Conference on Computer Vision and Pattern Recognition (CVPR)},
	  year={2017}
	}
	
Please contact me (yindaz AT cs DOT princeton DOT edu) if you have any problem in using the code/data/model.

## Testing
You need to specify the `-test_file` in [`config.lua`](./config.lua) with a list of file names pointing to the images you are testing. [`./image/`](./image/) provides an example.

We provide several pretrained models for testing. You can download them in [`./model/`](./model/).

Run
```
th main_test.lua -test_model ./model/sync_physic_nyufinetune.t7
```
and the result will be in [`./result/`](./result/). The estimated normal will be saved in 16-bit PNG format, where 0-65535 in R,G,B channel correspond to [-1, 1] for the X,Y,Z component of the normal vector. We use the camera coordinates defined as - X points to the camera right, Y points to the camera forward, and Z points to the camera upward. For example, right facing wall are very red, floor are very blue, and you rarely see green as it's parallel to the camera viewing direction.

## Training
You need to specify the `-train_file` in [`config.lua`](./config.lua) first.

Run
```
th main_train_single.lua -ps ./model/train_example
```
The training process will start, and snapshots will be saved as `./model/train_example_iter_xxx.t7`. To finetune a model, run
```
th main_train_single.lua -finetune -finetune_model ./model/train_example.t7 -ps ./model/train_finetune_example
```

## Data
To train on synthetic image, you can find the training data from http://pbrs.cs.princeton.edu. Specifically,
- `Color image`: http://pbrs.cs.princeton.edu/pbrs_release/data/mlt_v2.zip (278GB)
- `Surface normal ground truth`: http://pbrs.cs.princeton.edu/pbrs_release/data/normal_v2.zip (27GB)
- `Data list`: http://pbrs.cs.princeton.edu/pbrs_release/data/data_goodlist_v2.txt

To experiment on NYUv2 data,
- `Color image and ground truth`: http://pbrs.cs.princeton.edu/pbrs_release/nyu/nyu_data.zip. This file is converted using data from http://www.cs.nyu.edu/~deigen/dnl/ and http://cs.nyu.edu/~silberman/datasets/nyu_depth_v2.html
- `Training data list`: http://pbrs.cs.princeton.edu/pbrs_release/nyu/trainNdxs.txt
- `Testing data list`: http://pbrs.cs.princeton.edu/pbrs_release/nyu/testNdxs.txt

