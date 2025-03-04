# ReXNet

> [ReXNet: Rethinking Channel Dimensions for Efficient Model Design](https://arxiv.org/abs/2007.00992)

##  Introduction

ReXNets is a new model achieved based on parameterization. It utilizes a new search method for a channel configuration via piece-wise linear functions of block index.  The search space contains the conventions, and an effective channel configuration that can be parameterized by a linear function of the block index is used. ReXNets outperforms the recent lightweight models including NAS-based models and further showed remarkable fine-tuning performances on COCO object detection, instance segmentation, and fine-grained classifications.

## Results

Our reproduced model performance on ImageNet-1K is reported as follows.

<div align="center">

| Model           | Context   |  Top-1 (%)  | Top-5 (%)  | Params (M) | Recipe                                                                                   | Download                                                                         |
|-----------------|-----------|-------|-------|------------|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| rexnet_x09 | D910x8-G | 77.06 | 93.41    | 4.13       | [yaml](https://github.com/mindspore-lab/mindcv/blob/main/configs/rexnet/rexnet_x09_ascend.yaml) | [weights](https://download.mindspore.cn/toolkits/mindcv/rexnet/rexnet_09-da498331.ckpt) |
| rexnet_x10 | D910x8-G | 77.38 | 93.60    | 4.84       | [yaml](https://github.com/mindspore-lab/mindcv/blob/main/configs/rexnet/rexnet_x10_ascend.yaml) | [weights](https://download.mindspore.cn/toolkits/mindcv/rexnet/rexnet_10-c5fb2dc7.ckpt)   |
| rexnet_x13 | D910x8-G | 79.06 | 94.28 | 7.61       | [yaml](https://github.com/mindspore-lab/mindcv/blob/main/configs/rexnet/rexnet_x13_ascend.yaml) | [weights](https://download.mindspore.cn/toolkits/mindcv/rexnet/rexnet_13-a49c41e5.ckpt)   |
| rexnet_x15 | D910x8-G | 79.95 | 94.74  | 9.79       | [yaml](https://github.com/mindspore-lab/mindcv/blob/main/configs/rexnet/rexnet_x15_ascend.yaml) | [weights](https://download.mindspore.cn/toolkits/mindcv/rexnet/rexnet_15-37a931d3.ckpt)   |
| rexnet_x20 | D910x8-G | 80.64 | 94.99  | 16.45      | [yaml](https://github.com/mindspore-lab/mindcv/blob/main/configs/rexnet/rexnet_x20_ascend.yaml) | [weights](https://download.mindspore.cn/toolkits/mindcv/rexnet/rexnet_20-c5810914.ckpt)   |

</div>

#### Notes
- Context: Training context denoted as {device}x{pieces}-{MS mode}, where mindspore mode can be G - graph mode or F - pynative mode with ms function. For example, D910x8-G is for training on 8 pieces of Ascend 910 NPU using graph mode.
- Top-1 and Top-5: Accuracy reported on the validation set of ImageNet-1K.


## Quick Start

### Preparation

#### Installation
Please refer to the [installation instruction](https://github.com/mindspore-lab/mindcv#installation) in MindCV.

#### Dataset Preparation
Please download the [ImageNet-1K](https://www.image-net.org/challenges/LSVRC/2012/index.php) dataset for model training and validation.

### Training

* Distributed Training

It is easy to reproduce the reported results with the pre-defined training recipe. For distributed training on multiple Ascend 910 devices, please run

```shell
# distributed training on multiple GPU/Ascend devices
mpirun -n 8 python train.py --config configs/rexnet/rexnet_x09_ascend.yaml --data_dir /path/to/imagenet
```

> If the script is executed by the root user, the `--allow-run-as-root` parameter must be added to `mpirun`.

Similarly, you can train the model on multiple GPU devices with the above `mpirun` command.

For detailed illustration of all hyper-parameters, please refer to [config.py](https://github.com/mindspore-lab/mindcv/blob/main/config.py).

**Note:**  As the global batch size  (batch_size x num_devices) is an important hyper-parameter, it is recommended to keep the global batch size unchanged for reproduction or adjust the learning rate linearly to a new global batch size.

* Standalone Training

If you want to train or finetune the model on a smaller dataset without distributed training, please run:

```shell
# standalone training on a CPU/GPU/Ascend device
python train.py --config configs/rexnet/rexnet_x09_ascend.yaml --data_dir /path/to/dataset --distribute False
```

### Validation

To validate the accuracy of the trained model, you can use `validate.py` and parse the checkpoint path with `--ckpt_path`.

```shell
python validate.py -c configs/rexnet/rexnet_x09_ascend.yaml --data_dir /path/to/imagenet --ckpt_path /path/to/ckpt
```

### Deployment

To deploy online inference services with the trained model efficiently, please refer to the [deployment tutorial](https://github.com/mindspore-lab/mindcv/blob/main/tutorials/deployment.md).

## References

[1] Han D, Yun S, Heo B, et al. Rethinking channel dimensions for efficient model design[C]//Proceedings of the IEEE/CVF conference on Computer Vision and Pattern Recognition. 2021: 732-741.
