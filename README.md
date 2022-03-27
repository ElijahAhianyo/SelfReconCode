# SelfRecon: Self Reconstruction Your Digital Avatar from Monocular Video
### | [Project Page](https://jby1993.github.io/SelfRecon/) | [Paper](https://arxiv.org/abs/2201.12792) | 
![](doc/teaser.png)
This repository contains a pytorch implementation of "[SelfRecon: Self Reconstruction Your Digital Avatar from Monocular Video (CVPR 2022)](https://arxiv.org/abs/2201.12792)".<br/>
Authors: Boyi Jiang, Yang Hong, [Hujun Bao](http://www.cad.zju.edu.cn/home/bao/), [Juyong Zhang](http://staff.ustc.edu.cn/~juyong/).

This code is protected under patent. It can be only used for research purposes. 

## Requirements
- Python 3
- Pytorch3d (0.4.0, some compatibility issues may occur in higher versions of pytorch3d) 

Note: A GTX 3090 is recommended to run SelfRecon, make sure enough GPU memory if using other cards.
## Install
```bash
conda env create -f environment.yml
conda activate SelfRecon
bash install.sh
```
## Run on PeopleSnapshot Dataset
### Prepross
Download the [Dataset](https://graphics.tu-bs.de/people-snapshot) and unzip it to some ROOT. Run the following code to extract data for female-3-casual, for example.
```bash
python people_snapshot_process.py --root $ROOT/people_snapshot_public/female-3-casual --save_root $ROOT/female-3-casual
```
### Extract Normals
To enable our normal optimization, you have to install [PIFuHD](https://shunsukesaito.github.io/PIFuHD/) and [Lightweight Openpose](https://github.com/Daniil-Osokin/lightweight-human-pose-estimation.pytorch) in your $ROOT1 and $ROOT2 first. Then copy generate_normals.py and generate_boxs.py to $ROOT1 and $ROOT2 seperately, and run the following code to extract normals before running SelfRecon:
```bash
cd $ROOT2
python generate_boxs.py --data $ROOT/female-3-casual/imgs
cd $ROOT1
python generate_normals.py --imgpath $ROOT/female-3-casual/imgs
```
Then, run SelfRecon with the following code:
```bash
CUDA_VISIBLE_DEVICES=0 python train.py --gpu-ids 0 --conf config.conf --data $ROOT/female-3-casual --save-folder result
```
The results locate in $ROOT/female-3-casual/result



## Inference
Run the following code to generate rendered meshes and images.
```bash
CUDA_VISIBLE_DEVICES=3 python infer.py --gpu-ids 0 --rec-root $ROOT/female-3-casual/result/ --C
```
## Acknowledgement

Here are some great resources we benefit or utilize from:
- [MarchingCubes_CppCuda](https://github.com/WanquanF/MarchingCubes_CppCuda) supplies the GPU MC module (MCGpu)
- [MonoPort](https://github.com/Project-Splinter/MonoPort) and [ImplicitSegCUDA](https://github.com/Project-Splinter/ImplicitSegCUDA/tree/master/implicit_seg/cuda) constructed our Marching Cubes accelerating (MCAcc)
- [SMPL](https://smpl.is.tue.mpg.de/) for Parametric Body Representation
- [IDR](https://github.com/lioryariv/idr) for Neural Implicit Reconstruction
- [PyTorch3D](https://github.com/facebookresearch/pytorch3d) for Differential Explicit Rendering

This research was supported by National Natural Science Foundation of China (No. 62122071), the Youth Innovation Promotion Association CAS (No. 2018495), ``the Fundamental Research Funds for the Central Universities''(No. WK3470000021).



  <!-- citing -->
  <div class="container">
      <div class="row ">
          <div class="col-12">
              <h2>Citation</h2>
              <pre style="background-color: #e9eeef;padding: 1.25em 1.5em"><code>@inproceedings{jiang2022selfrecon,
  author    = {Boyi Jiang and Yang Hong and Hujun Bao and Juyong Zhang},
  title     = {SelfRecon: Self Reconstruction Your Digital Avatar from Monocular Video},
  booktitle = {{IEEE/CVF} Conference on Computer Vision and Pattern Recognition (CVPR)},
  year      = {2022}
}</code></pre>
        </div>
      </div>
  </div>
