# TRADES-random-smoothing

This is the code for the paper "Random Smoothing Might be Unable to Certify ![](http://latex.codecogs.com/gif.latex?\ell_\infty) Robustness for High-Dimensional Images".

TRADES + Randomized smoothing achieves SOTA certified adversarial robustness in ![](http://latex.codecogs.com/gif.latex?\ell_\infty) norm at radius 2/255.

## Prerequisites
* Python (3.6.4)
* Pytorch (0.4.1)
* CUDA
* numpy

## Install
We suggest to install the dependencies using Anaconda or Miniconda. Here is an exemplary command:
```
$ wget https://repo.anaconda.com/archive/Anaconda3-5.1.0-Linux-x86_64.sh
$ bash Anaconda3-5.1.0-Linux-x86_64.sh
$ source ~/.bashrc
$ conda install pytorch=0.4.1
```
## TRADES+random smooth: A New Training Method for Certifiable Robustness

We used TRADES to train a soft-random-smoothing classifier by injecting Gaussian noise. The method minimizes empirical risk of a regularized surrogate loss L(.,.) (e.g., the cross-entropy loss) with Gaussian noise:
![](http://latex.codecogs.com/gif.latex?\min_{f}\mathbb{E}_{X,Y}\mathbb{E}_{\eta\sim\mathcal{N}(0,\sigma^2I)}\left[\mathcal{L}(f(X+\eta),Y)+\beta\max_{X'\in\mathbb{B}_2(X,\epsilon)}\mathcal{L}(f(X+\eta),f(X'+\eta))\right].)

## Running Demos

### Certified ![](http://latex.codecogs.com/gif.latex?\ell_\infty) robustness:

* Train ResNet-110 model on CIFAR10:
```bash
  $ python code/train_trades.py --epsilon 0.435 --imagesize 32 --noise_sd 0.12 --model-dir 'checkpoints/model_noise12_size32_l2trades0435_beta6'
```

* Certify ResNet-110 model on CIFAR10:
```bash
  $ python code/certify_trades.py cifar10 --imagesize 32 --sigma 0.12 --outfile 'results/output_noise12_size32_l2trades0435_beta6_full' --skip 1 --batch 10000 --load_dir 'checkpoints/model_noise12_size32_l2trades0435_beta6/model-wideres-epoch.pt'
```

### Effectiveness of lower bound:
* Train ResNet-110 models of varying input sizes on CIFAR10:
```bash
  $ python code/train_trades.py --epsilon 0.435 --imagesize 32 --noise_sd 0.12 --model-dir 'checkpoints/model_noise12_size32_l2trades0435_beta6'
  $ python code/train_trades.py --epsilon 0.6525 --imagesize 48 --noise_sd 0.18 --model-dir 'checkpoints/model_noise18_size48_l2trades06525_beta6'
  $ python code/train_trades.py --epsilon 0.87 --imagesize 64 --noise_sd 0.24 --model-dir 'checkpoints/model_noise24_size64_l2trades087_beta6'
```

* Certify ResNet-110 models of varying input sizes on CIFAR10:
```bash
  $ python code/certify_trades.py cifar10 --imagesize 32 --sigma 0.12 --outfile 'results/output_noise12_size32_l2trades0435_beta6' --skip 20 --batch 10000 --load_dir 'checkpoints/model_noise12_size32_l2trades0435_beta6/model-wideres-epoch.pt'
  $ python code/certify_trades.py cifar10 --imagesize 48 --sigma 0.18 --outfile 'results/output_noise18_size48_l2trades06525_beta6' --skip 20 --batch 10000 --load_dir 'checkpoints/model_noise18_size48_l2trades06525_beta6/model-wideres-epoch.pt'
  $ python code/certify_trades.py cifar10 --imagesize 64 --sigma 0.24 --outfile 'results/output_noise24_size64_l2trades087_beta6' --skip 20 --batch 10000 --load_dir 'checkpoints/model_noise24_size64_l2trades087_beta6/model-wideres-epoch.pt'
```

* Analyze results:
```bash
  $ python code/analyze.py 
```

<p align="center">
    <img src="results/vary_dim_cifar10_trades.png" width="450"\>
</p>
<p align="center">
