# Team DVision


## Installation

```bash
cd Depth-Anything
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

## Test file creation

Change the path in ``dataset_dir`` to the directory having the testing dataset. 

For Example: ``dataset_dir = "./dataset/test_mono_nogt"``

* Note: Directory structure should be same as of test_mono_nogt.

```bash
python dataset_paths.py 
```

The ``test.txt`` file corresponding to the test dataset will be created in ``dataset_paths`` folder. Pass this ``test.txt`` file path to ``img-path`` argument of ``inference`` command. 

The test dataset must be present in dataset folder. 

A sample test.txt file is already present in dataset_paths


## Inference

```bash
python run.py [--grayscale]
```
Arguments:
- ``--img-path``: you can either 1) point it to an image directory storing all interested images, 2) point it to a single image, or 3) point it to a text file storing all image paths.
- ``-p``: path to models checkpoints. (Present in checkpoints_new folder)
- ``--outdir``: path to dir to save the output depths
- ``-width``: width of output depth map
- ``-height``: height of output depth map
- ``--grayscale``: is set to save the grayscale depth map. Without it, by default, we apply a color palette to the depth map.

For example:
```bash
python run.py --img-path dataset_paths/test.txt --outdir depth_vis --grayscale
```
The outputs will be saved in ``depth_vis`` folder  

## Training dataset creation

```bash
python dataset_creation.py --dataset_txt ./dataset_paths/train_data.txt
```
Arguments:
- ``--dataset_txt``: path to dataset txt having format <image_path.png> <depth_path.npy> <mask_path.png>
- ``--save_dir``: dir path to save the patched dataset

Output:
Dataset will be created in dataset/train folder. 

A train_extended.txt file will be created in dataset_paths having paths of patched dataset. 

Pass this text file in ``train_txt`` argument of training command


## Training

```bash
python train.py --train_txt dataset_paths/train_extended.txt
```
Arguments:
- ``--train_txt``: path to train txt
- ``--checkpoints-dir``: dir path to save model's checkpoints
- ``--should-log``: wandb log 1) 1: enable 2) 0: disable
- ``--batch-size``: batch size
- ``--w``: width of target depth map
- ``--h``: height of target depth map
- ``--epochs``: no of epoch
- ``--lr``: learning rate

For example:
```bash
python train.py --train_txt dataset_paths/train_extended.txt --should-log 0 --batch_size 2 --epochs 10 
```