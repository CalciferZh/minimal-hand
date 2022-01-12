# Minimal Hand

A minimal solution to hand motion capture from a single color camera at over 100fps.
Easy to use, plug to run.

![teaser](teaser.gif)

This project provides the core components for hand motion capture:
1. estimating joint **locations** from a monocular RGB image (DetNet)
1. estimating joint **rotations** from locations (IKNet)

We focus on:
1. ease of use (all you need is a webcam)
1. time efficiency (on our 1080Ti, 8.9ms for DetNet, 0.9ms for IKNet)
1. robustness to occlusion, hand-object interaction, fast motion, changing scale and view point

Some links:
[\[video\]](https://youtu.be/OIRulRoBdL4)
[\[paper\]](https://calciferzh.github.io/files/zhou2020monocular.pdf)
[\[supp doc\]](https://calciferzh.github.io/files/zhou2020monocular_supp.pdf)
[\[webpage\]](https://calciferzh.github.io/publications/zhou2020monocular)

The author is too busy to collect the training code for release.
On the other hand, it should not be difficult to implement the training part.
Feel free to open an issue for any encountered problems.

### Pytorch Version
[Here](https://github.com/MengHao666/Minimal-Hand-pytorch) is a pytorch version implemented by @MengHao666.
I didn't personally check it but I believe it worth trying.
Many thanks to @MengHao666 !

### With Unity
[Here](https://github.com/vinnik-dmitry07/minimal-hand) is a project that connects this repo to unity.
It looks very cool and many thanks to @vinnik-dmitry07 !


## Usage

### Install dependencies
Please check `requirements.txt`. All dependencies are available via pip and conda.

### Prepare MANO hand model
1. Download MANO model from [here](https://mano.is.tue.mpg.de/) and unzip it.
1. In `config.py`, set `OFFICIAL_MANO_PATH` to the **left hand** model.
1. Run `python prepare_mano.py`, you will get the converted MANO model that is compatible with this project at `config.HAND_MESH_MODEL_PATH`.

### Prepare pre-trained network models
1. Download models from [here](https://github.com/CalciferZh/minimal-hand/releases/download/v1/cvpr_2020_hand_model_v1.zip).
1. Put `detnet.ckpt.*` in `model/detnet`, and `iknet.ckpt.*` in `model/iknet`.
1. Check `config.py`, make sure all required files are there.

### Run the demo for webcam input
1. `python app.py`
1. Put your **right hand** in front of the camera. The pre-trained model is for left hand, but the input would be flipped internally.
1. Press `ESC` to quit.
1. Although the model is robust to variant scales, most ideally the image should be 1.3x larger than the hand bounding box. A good bounding box may result in better accuracy. You can track the bounding box with the 2D predictions of the model.

We found that the model may fail on some "simple" poses. We think this is because such poses were no presented in the training data. We are working on a v2 version with further extended data to tackle this problem.

### Use the models in your project
Please check `wrappers.py`.

## IKNet Alternative
We also provide an optimization-based IK solver [here](https://github.com/CalciferZh/Minimal-IK).

## Dataset
The detection model is trained with following datasets:
* [CMU HandDB](http://domedb.perception.cs.cmu.edu/handdb.html)
* [Rendered Handpose Dataset](https://lmb.informatik.uni-freiburg.de/resources/datasets/RenderedHandposeDataset.en.html)
* [GANerated Hands Dataset](https://handtracker.mpi-inf.mpg.de/projects/GANeratedHands/GANeratedDataset.htm)

The IK model is trained with the poses shipped with [MANO](https://mano.is.tue.mpg.de/).

## Citation

This is the official implementation of the paper "Monocular Real-time Hand Shape and Motion Capture using Multi-modal Data" (CVPR 2020).

The quantitative numbers reported in the paper can be found in `plot.py`.

If you find the project helpful, please consider citing us:
```
@InProceedings{zhou2020monocular,
  author = {Zhou, Yuxiao and Habermann, Marc and Xu, Weipeng and Habibie, Ikhsanul and Theobalt, Christian and Xu, Feng},
  title = {Monocular Real-Time Hand Shape and Motion Capture Using Multi-Modal Data},
  booktitle = {IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
  month = {June},
  year = {2020}
}
```
