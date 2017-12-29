# PyTorch Image Classification

Following papers are implemented using PyTorch.

* ResNet (1512.03385)
* ResNet-preact (1603.05027)
* WRN (1605.07146)
* DenseNet (1608.06993)
* PyramidNet (1610.02915)
* ResNeXt (1611.05431)
* shake-shake (1705.07485)
* Cutout (1708.04552)
* Random Erasing (1708.04896)
* SENet (1709.01507)
* Mixup (1710.09412)



## Requirements

* Python 3.6.3
* PyTorch
* torchvision
* [tensorboard-pytorch]( https://github.com/lanpa/tensorboard-pytorch ) (optional)



## Usage

```
$ ./main.py --arch resnet_preact --depth 56 --outdir results
```

### Use Cutout

```
$ ./main.py --arch resnet_preact --depth 56 --outdir results --use_cutout
```

### Use RandomErasing

```
$ ./main.py --arch resnet_preact --depth 56 --outdir results --use_random_erasing
```

### Use Mixup

```
$ ./main.py --arch resnet_preact --depth 56 --outdir results --use_mixup
```

### Use cosine annealing

```
$ ./main.py --arch wrn --outdir results --scheduler cosine
```



## Results

### Results on CIFAR-10 using almost same settings as papers

![](figures/cifar10/test_errors.png)

| Model                                  | Test Error (median of 3 runs) | Test Error (in paper)                 | Training Time |
|:---------------------------------------|:-----------------------------:|:-------------------------------------:|--------------:|
| VGG-like (depth 15, w/ BN, channel 64) | 7.29                          | N/A                                   |   1h20m       |
| ResNet-110                             | 6.52                          | 6.43 (best), 6.61 ± 0.16             |   3h06m       |
| ResNet-preact-110                      | 6.47                          | 6.37 (median of 5 runs)               |   3h05m       |
| ResNet-preact-164 bottleneck           | 5.90                          | 5.46 (median of 5 runs)               |   4h01m       |
| ResNet-preact-1001 bottleneck          |                               | 4.62 (median of 5 runs), 4.69 ± 0.20 |               |
| WRN-28-10                              | 4.03                          | 4.00 (median of 5 runs)               |  16h10m       |
| WRN-28-10 w/ dropout                   |                               | 3.89 (median of 5 runs)               |               |
| DenseNet-100 (k=12)                    |                               | 4.10 (1 run)                          |               |
| DenseNet-100 (k=24)                    |                               | 3.74 (1 run)                          |               |
| DenseNet-BC-100 (k=12)                 | 4.69                          | 4.51 (1 run)                          |  15h20m       |
| DenseNet-BC-250 (k=24)                 |                               | 3.62 (1 run)                          |               |
| DenseNet-BC-190 (k=40)                 |                               | 3.46 (1 run)                          |               |
| PyramidNet-110 (alpha=84)              | 4.40                          | 4.26 ± 0.23                          |  11h40m       |
| PyramidNet-110 (alpha=270)             | 3.92 (1 run)                  | 3.73 ± 0.04                          |  24h12m*      |
| PyramidNet-164 bottleneck (alpha=270)  |                               | 3.48 ± 0.20                          |               |
| PyramidNet-272 bottleneck (alpha=200)  |                               | 3.31 ± 0.08                          |               |
| ResNeXt-29 4x64d                       | 3.89                          | ~3.75 (from Figure 7)                 |  31h17m       |
| ResNeXt-29 8x64d                       | 3.97 (1 run)                  | 3.65 (average of 10 runs)             |  42h50m*      |
| ResNeXt-29 16x64d                      |                               | 3.58 (average of 10 runs)             |               |
| shake-shake-26 2x32d (S-S-I)           | 3.68                          | 3.55 (average of 3 runs)              |  33h49m       |
| shake-shake-26 2x64d (S-S-I)           |                               | 2.98 (average of 3 runs)              |               |
| shake-shake-26 2x96d (S-S-I)           | 2.90 (1 run)                  | 2.86 (average of 5 runs)              | 101h32m*      |


#### Notes

* Differences with papers in training settings:
    * Trained WRN-28-10 with batch size 64 (128 in paper).
    * Trained DenseNet-BC-100 (k=12) with batch size 32 and initial learning rate 0.05 (batch size 64 and initial learning rate 0.1 in paper).
    * Trained ResNeXt-29 4x64d with a single GPU, batch size 32 and initial learning rate 0.025 (8 GPUs, batch size 128 and initial learning rate 0.1 in paper).
    * Trained shake-shake 26 2x32d (S-S-I) with a single GPU (2 GPUs in paper).
* Test errors reported above are the ones at last epoch.
* Experiments with only 1 run are done on different computer from the one used for experiments with 3 runs.

#### VGG-like

```
$ python -u main.py --arch vgg --seed 7 --outdir results/vgg_15_BN_64/00
```

![](figures/cifar10/VGG-15_BN_64.png)

#### ResNet

```
$ python -u main.py --arch resnet --depth 110 --block_type basic --seed 7 --outdir results/resnet_basic_110/00
```

![](figures/cifar10/ResNet-110_basic.png)

#### ResNet-preact

```
$ python -u main.py --arch resnet_preact --depth 110 --block_type basic --seed 7 --outdir results/resnet_preact_basic_110/00
```

![](figures/cifar10/ResNet-preact-110_basic.png)


```
$ python -u main.py --arch resnet_preact --depth 164 --block_type bottleneck --seed 7 --outdir results/resnet_preact_bottleneck_164/00
```

![](figures/cifar10/ResNet-preact-164_bottleneck.png)

#### WRN

```
$ python -u main.py --arch wrn --depth 28 --widening_factor 10 --seed 7 --outdir results/wrn_28_10/00
```

![](figures/cifar10/WRN-28-10.png)

#### DenseNet

```
$ python -u main.py --arch densenet --depth 100 --block_type bottleneck --growth_rate 12 --compression_rate 0.5 --batch_size 32 --base_lr 0.05 --seed 7 --outdir results/densenet_BC_100_12/00
```

![](figures/cifar10/DenseNet-BC-100_k_12.png)

#### PyramidNet

```
$ python -u main.py --arch pyramidnet --depth 110 --block_type basic --pyramid_alpha 84 --seed 7 --outdir results/pyramidnet_basic_110_84/00
```

![](figures/cifar10/PyramidNet-110_alpha_84.png)

```
$ python -u main.py --arch pyramidnet --depth 110 --block_type basic --pyramid_alpha 270 --seed 7 --outdir results/pyramidnet_basic_110_270/00
```

![](figures/cifar10/PyramidNet-110_alpha_270.png)

#### ResNeXt

```
$ python -u main.py --arch resnext --depth 29 --cardinality 4 --base_channels 64 --batch_size 32 --base_lr 0.025 --seed 7 --outdir results/resnext_29_4x64d/00
```

![](figures/cifar10/ResNeXt-29_4x64d.png)

```
$ python -u main.py --arch resnext --depth 29 --cardinality 8 --base_channels 64 --batch_size 64 --base_lr 0.05 --seed 7 --outdir results/resnext_29_8x64d/00
```

![](figures/cifar10/ResNeXt-29_8x64d.png)

#### shake-shake

```
$ python -u main.py --arch shake_shake --depth 26 --base_channels 32 --shake_forward True --shake_backward True --shake_image True --seed 7 --outdir results/shake_shake_26_2x32d_SSI/00
```

![](figures/cifar10/shake-shake-26_2x32d.png)

```
$ python -u main.py --arch shake_shake --depth 26 --base_channels 96 --shake_forward True --shake_backward True --shake_image True --seed 7 --outdir results/shake_shake_26_2x96d_SSI/00
```

![](figures/cifar10/shake-shake-26_2x96d.png)



## Experiments

### Experiment on residual units, learning rate scheduling, and data augmentation

In this experiment, the effects of the following on classification accuracy are investigated:

* PyramidNet-like residual units
* Cosine annealing of learning rate
* Cutout
* Random Erasing
* Mixup
* Preactivation of shortcuts after downsampling

ResNet-preact-56 is trained on CIFAR-10 with initial learning rate 0.2 in this experiment.

#### Note

* PyramidNet paper (1610.02915) showed that removing first ReLU in residual units and adding BN after last convolutions in residual units both improve classification accuracy.
* SGDR paper (1608.03983) showed cosine annealing improves classification accuracy even without restarting.

#### Results

* PyramidNet-like units works.
    * It might be better not to preactivate shortcuts after downsampling when using PyramidNet-like units.
* Cosine annealing slightly improves accuracy.
* Cutout, RandomErasing, and Mixup all work great.
    * Mixup needs longer training.


![](figures/experiments_resnet/test_errors.png)

| Model                                                             | Test Error (median of 5 runs) | Training Time |
|:------------------------------------------------------------------|:-----------------------------:|--------------:|
| w/ 1st ReLU, w/o last BN, preactivate shortcut after downsampling | 6.45                          |    95 min     |
| w/ 1st ReLU, w/o last BN                                          | 6.47                          |    95 min     |
| w/o 1st ReLU, w/o last BN                                         | 6.14                          |    89 min     |
| w/ 1st ReLU, w/ last BN                                           | 6.43                          |   104 min     |
| w/o 1st ReLU, w/ last BN                                          | 5.85                          |    98 min     |
| w/o 1st ReLU, w/ last BN, preactivate shortcut after downsampling | 6.27                          |    98 min     |
| w/o 1st ReLU, w/ last BN, Cosine annealing                        | 5.72                          |    98 min     |
| w/o 1st ReLU, w/ last BN, Cutout                                  | 4.96                          |    98 min     |
| w/o 1st ReLU, w/ last BN, RandomErasing                           | 5.22                          |    98 min     |
| w/o 1st ReLU, w/ last BN, Mixup (300 epochs)                      | 5.11                          |   191 min     |


##### preactivate shortcut after downsampling
```
$ python -u main.py --arch resnet_preact --depth 56 --block_type basic --base_lr 0.2 --preact_stage '[true, true, true]' --remove_first_relu false --add_last_bn false --seed 7 --outdir results/experiments/00_preact_after_downsampling/00
```

![](figures/experiments_resnet/w_1st_ReLU_wo_last_BN_preactivate_after_downsampling.png)


##### w/ 1st ReLU, w/o last BN
```
$ python -u main.py --arch resnet_preact --depth 56 --block_type basic --base_lr 0.2 --preact_stage '[true, false, false]' --remove_first_relu false --add_last_bn false --seed 7 --outdir results/experiments/01_w_relu_wo_bn/00
```

![](figures/experiments_resnet/w_1st_ReLU_wo_last_BN.png)

##### w/o 1st ReLU, w/o last BN
```
$ python -u main.py --arch resnet_preact --depth 56 --block_type basic --base_lr 0.2 --preact_stage '[true, false, false]' --remove_first_relu true --add_last_bn false --seed 7 --outdir results/experiments/02_wo_relu_wo_bn/00
```

![](figures/experiments_resnet/wo_1st_ReLU_wo_last_BN.png)

##### w/ 1st ReLU, w/ last BN
```
$ python -u main.py --arch resnet_preact --depth 56 --block_type basic --base_lr 0.2 --preact_stage '[true, false, false]' --remove_first_relu false --add_last_bn true --seed 7 --outdir results/experiments/03_w_relu_w_bn/00
```

![](figures/experiments_resnet/w_1st_ReLU_w_last_BN.png)

##### w/o 1st ReLU, w/ last BN
```
$ python -u main.py --arch resnet_preact --depth 56 --block_type basic --base_lr 0.2 --preact_stage '[true, false, false]' --remove_first_relu true --add_last_bn true --seed 7 --outdir results/experiments/04_wo_relu_w_bn/00
```

![](figures/experiments_resnet/wo_1st_ReLU_w_last_BN.png)

##### w/o 1st ReLU, w/ last BN, preactivate shortcut after downsampling
```
$ python -u main.py --arch resnet_preact --depth 56 --block_type basic --base_lr 0.2 --preact_stage '[true, true, true]' --remove_first_relu true --add_last_bn true --seed 7 --outdir results/experiments/05_preact_after_downsampling/00
```

![](figures/experiments_resnet/wo_1st_ReLU_w_last_BN_preactivate_after_downsampling.png)

##### w/o 1st ReLU, w/ last BN, cosine annealing
```
$ python -u main.py --arch resnet_preact --depth 56 --block_type basic --base_lr 0.2 --preact_stage '[true, false, false]' --remove_first_relu true --add_last_bn true --scheduler cosine --seed 7 --outdir results/experiments/06_cosine_annealing/00
```

![](figures/experiments_resnet/wo_1st_ReLU_w_last_BN_Cosine_annealing.png)

##### w/o 1st ReLU, w/ last BN, Cutout
```
$ python -u main.py --arch resnet_preact --depth 56 --block_type basic --base_lr 0.2 --preact_stage '[true, false, false]' --remove_first_relu true --add_last_bn true --use_cutout --seed 7 --outdir results/experiments/07_cutout/00
```

![](figures/experiments_resnet/wo_1st_ReLU_w_last_BN_Cutout.png)

##### w/o 1st ReLU, w/ last BN, RandomErasing
```
$ python -u main.py --arch resnet_preact --depth 56 --block_type basic --base_lr 0.2 --preact_stage '[true, false, false]' --remove_first_relu true --add_last_bn true --use_random_erasing --seed 7 --outdir results/experiments/08_random_erasing/00
```

![](figures/experiments_resnet/wo_1st_ReLU_w_last_BN_Random_Erasing.png)

##### w/o 1st ReLU, w/ last BN, Mixup
```
$ python -u main.py --arch resnet_preact --depth 56 --block_type basic --base_lr 0.2 --preact_stage '[true, false, false]' --remove_first_relu true --add_last_bn true --use_mixup --seed 7 --outdir results/experiments/09_mixup/00
```

![](figures/experiments_resnet/wo_1st_ReLU_w_last_BN_Mixup.png)



## References

* He, Kaiming, et al. "Deep residual learning for image recognition." Proceedings of the IEEE conference on computer vision and pattern recognition. 2016. [arXiv:1512.03385]( https://arxiv.org/abs/1512.03385 )
* He, Kaiming, et al. "Identity mappings in deep residual networks." European Conference on Computer Vision. Springer International Publishing, 2016. [arXiv:1603.05027]( https://arxiv.org/abs/1603.05027 ), [Torch implementation]( https://github.com/KaimingHe/resnet-1k-layers )
* Sergey Zagoruyko and Nikos Komodakis. Wide Residual Networks. In Richard C. Wilson, Edwin R. Hancock and William A. P. Smith, editors, Proceedings of the British Machine Vision Conference (BMVC), pages 87.1-87.12. BMVA Press, September 2016. [arXiv:1605.07146]( https://arxiv.org/abs/1605.07146 ), [Torch implementation]( https://github.com/szagoruyko/wide-residual-networks )
* Loshchilov, Ilya, and Frank Hutter. "Sgdr: Stochastic gradient descent with warm restarts." In International Conference on Learning Representations, 2017. [arXiv:1608.03983]( https://arxiv.org/abs/1608.03983 ), [Lasagne implementation]( https://github.com/loshchil/SGDR )
* Huang, Gao, et al. "Densely connected convolutional networks." The IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2017, pp. 4700-4708. [arXiv:1608.06993]( https://arxiv.org/abs/1608.06993 ), [Torch implementation]( https://github.com/liuzhuang13/DenseNet )
* Han, Dongyoon, Jiwhan Kim, and Junmo Kim. "Deep pyramidal residual networks." The IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2017, pp. 5927-5935. [arXiv:1610.02915]( https://arxiv.org/abs/1610.02915 ), [Torch implementation]( https://github.com/jhkim89/PyramidNet ), [Caffe implementation]( https://github.com/jhkim89/PyramidNet-caffe ), [PyTorch implementation]( https://github.com/dyhan0920/PyramidNet-PyTorch )
* Xie, Saining, et al. "Aggregated residual transformations for deep neural networks." The IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2017, pp. 1492-1500. [arXiv:1611.05431]( https://arxiv.org/abs/1611.05431 ), [Torch implementation]( https://github.com/facebookresearch/ResNeXt )
* Gastaldi, Xavier. "Shake-Shake regularization." In International Conference on Learning Representations, 2017. [arXiv:1705.07485]( https://arxiv.org/abs/1705.07485 ), [Torch implementation]( https://github.com/xgastaldi/shake-shake )
* DeVries, Terrance, and Graham W. Taylor. "Improved regularization of convolutional neural networks with cutout." arXiv preprint arXiv:1708.04552 (2017). [arXiv:1708.04552]( https://arxiv.org/abs/1708.04552 ), [PyTorch implementation]( https://github.com/uoguelph-mlrg/Cutout )
* Zhong, Zhun, et al. "Random Erasing Data Augmentation." arXiv preprint arXiv:1708.04896 (2017). [arXiv:1708.04896]( https://arxiv.org/abs/1708.04896 ), [PyTorch implementation]( https://github.com/zhunzhong07/Random-Erasing )
* Hu, Jie, Li Shen, and Gang Sun. "Squeeze-and-excitation networks." arXiv preprint arXiv:1709.01507 (2017). [arXiv:1709.01507]( https://arxiv.org/abs/1709.01507 )
* Hongyi Zhang, Moustapha Cisse, Yann N. Dauphin, David Lopez-Paz. "mixup: Beyond Empirical Risk Minimization." arXiv preprint arXiv:1710.09412 (2017). [arXiv:1710.09412]( https://arxiv.org/abs/1710.09412 )

