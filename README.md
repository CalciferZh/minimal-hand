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

The release of training code is still in progress.

## Usage

### Install dependencies
Please check `requirements.txt`. All dependencies are available via pip and conda.

### Prepare MANO hand model
1. Download MANO model from [here](https://mano.is.tue.mpg.de/) and unzip it.
1. In `config.py`, set `OFFICIAL_MANO_PATH` to the **left hand** model.
1. Run `python prepare_mano.py`, you will get the converted MANO model that is compatible with this project at `config.HAND_MESH_MODEL_PATH`.

### Prepare pre-trained network models
1. Download models from [here](https://drive.google.com/open?id=1ZnDYF9rHKbef27tiHkWrLznqe18le7ol).
1. Put `detnet.ckpt.*` in `model/detnet`, and `iknet.ckpt.*` in `model/iknet`.
1. Check `config.py`, make sure all required files are there.

### Run the demo for webcam input
1. `python app.py`
1. Put your **right hand** in front of the camera. The pre-trained model is for left hand, but the input would be flipped internally.
1. Press `ESC` to quit.
1. Although the model is robust to variant scales, most ideally the image should be 1.3x larger than the hand boudning box. A good bounding box may result in better accuracy. You can track the bounding box with the 2D predictions of the model.

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
@inproceedings{zhou2020monocular,
  title={Monocular Real-time Hand Shape and Motion Capture using Multi-modal Data},
  author={Zhou, Yuxiao and Habermann, Marc and Xu, Weipeng and Habibie, Ikhsanul and Theobalt, Christian and Xu, Feng},
  booktitle={Proceedings of the IEEE International Conference on Computer Vision},
  pages={0--0},
  year={2020}
}
```