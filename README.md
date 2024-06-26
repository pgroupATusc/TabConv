# TabConv: Low-Computation CNN Inference via Table Lookups 

<a href="https://zenodo.org/doi/10.5281/zenodo.10983501"><img src="https://zenodo.org/badge/607371605.svg" alt="DOI"></a>

## Abstract
Convolutional Neural Networks (CNNs) have demonstrated remarkable ability throughout the field of computer vision. 
However, CNN inference requires a large number of arithmetic operations, making them expensive to deploy in hardware.
Current approaches alleviate this issue by developing hardware-supported, algorithmic processes to simplify spatial convolution functions. 
However, these methods still heavily rely on matrix multiplication, leading to significant computational overhead.
To bridge the gap between hardware, algorithmic acceleration, and approximate matrix multiplication, we propose \textit{TabConv}, a novel, table-based approximation for convolution to significantly reduce arithmetic operations during inference.
Additionally, we introduce a priority masking technique based on cosine similarity to select layers for table-based approximation, thereby maintaining the model performance.
We evaluate our approach on popular CNNs: ResNet-18, ResNet-34, and NetworkInNetwork (NIN).
TabConv preserves over 93\% of the original model's performance while reducing arithmetic operations by 36.5\%, 25.8\%, and 99.4\% for ResNet-18 on CIFAR-10, CIFAR-100, and MNIST, respectively, 35.6\% and 99.3\% for ResNet-34 on CIFAR-10 and MNIST, and 98.9\% for NIN on MNIST, achieving low-computation inference.

## Installation
Begin by cloning the repository and setting up the environment:
```bash
git clone www.github.com/neeleshg23/TabConv.git
cd TabConv
conda env create -f env.yaml 
conda activate rapid
```

By default, the data directory is set to `/data/CV_Datasets`.

Edit `1_NN/main.py` line 33 to correct this. 

Edit `2_AMM_GPU/main.py` line 86 to correct this. 

Edit `3_AMM_CPU/main.py` line 82 to correct this. 

## Experiment Workflow
### Dataset Command Abbreviations
| Dataset Name | Abbreviation |
|--------------|--------------|
| CIFAR-10     | c10          |
| CIFAR-100    | c100         |
| MNIST        | m            |

### Model Command Abbreviations
| Model Name       | Abbreviation | Number of Switches |
|------------------|--------------|--------------------|
| ResNet-18        | r18          | 10                 |
| ResNet-34        | r34          | 19                 |
| NetworkInNetwork | n            | 9                  |

### Training Neural Networks
To train neural networks, navigate to the `1_NN` directory and run the `main.py` script with appropriate parameters.
```bash
cd 1_NN
python main.py -h
```
Below is the help message displayed.
```
usage: main.py [-h] --dataset DATASET --model MODEL --gpu GPU

optional arguments:
  -h, --help            show this help message and exit
  --dataset DATASET, -d DATASET
                        Dataset abbreviation
  --model MODEL, -m MODEL
                        Model to train abbreviation
  --gpu GPU, -g GPU     GPU number
```

#### Example Usage
```bash
python main.py --dataset c10 --model r18 --gpu 0
```
There is a sample model state dictionary for ResNet-18 on CIFAR-10 in `0_RES/1_NN`.

### Training Table-Based Models
To train table-based approximations of NNs, navigate to the `2_AMM_GPU` directory for GPU acceleration and `3_AMM_CPU` directory and run the `main.py` script with appropriate parameters on the GPU.

Below is the help message displayed.
```
usage: main.py [-h] --model MODEL --dataset DATASET --gpu GPU --ncodebook NCODEBOOK --kcentroid KCENTROID --train TRAIN --test TEST --switch SWITCH

optional arguments:
  -h, --help            show this help message and exit
  --model MODEL, -m MODEL
                        Model abbreviation
  --dataset DATASET, -d DATASET
                        Dataset name
  --gpu GPU, -g GPU     GPU number
  --ncodebook NCODEBOOK, -n NCODEBOOK
                        Number of subspaces per MM
  --kcentroid KCENTROID, -k KCENTROID
                        Number of centroids per subspace
  --train TRAIN, -tr TRAIN
                        Number of train images
  --test TEST, -te TEST
                        Number of train images
  --switch SWITCH, -s SWITCH
                        String of "Number of Switches" boolean values as 0/1 with comma separation
```

#### Example Usage with Pretrained Model Weights
##### GPU Version:
```bash
python main.py --model r18 --dataset c10 --ncodebook 8 --kcentroid 8192 --train 1000 --test 500 --switch 1,1,1,1,1,1,1,1,1,1 --gpu 0
```
##### CPU Version
```bash
python main.py --model r18 --dataset c10 --ncodebook 8 --kcentroid 8192 --train 1000 --test 500 --switch 1,1,1,1,1,1,1,1,1,1
```

### Results
Accuracy results are found in `/0_RES`.

### Citation
```
@inproceedings{gupta2023tabconv,
  title     = {TabConv: Low-Computation CNN Inference via Table Lookups},
  author    = {Gupta, Neelesh and Kannan, Narayanan and Zhang, Pengmiao and Prasanna, Viktor},
  year      = {2024},
  doi       = {10.1145/3649153.3649212},
  booktitle = {Proceedings of the 21st ACM International Conference on Computing Frontiers},
  series    = {CF '24},
  keywords  = {convolutional neural network, table lookup, product quantization},
  location  = {Ischia, Italy},
  publisher = {Association for Computing Machinery},
  address   = {New York, NY, USA},
  isbn = {9798400705977},
  url = {https://doi.org/10.1145/3649153.3649212},
  numpages = {8}
}
```
