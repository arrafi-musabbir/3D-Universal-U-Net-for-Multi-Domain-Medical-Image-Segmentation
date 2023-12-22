My modified version that fixes the issues of the original repo to run it now with python3.10 at 2023
#### [3D U<sup>2</sup>-Net: A 3D Universal U-Net for Multi-Domain Medical Image Segmentation](https://link.springer.com/chapter/10.1007%2F978-3-030-32245-8_33) by Chao Huang, Qingsong Yao, Hu Han, Shankuan Zhu, Shaohua Zhou. This is a code repo of the paper early accepted by MICCAI2019.

**Abstract**. Fully convolutional neural networks like U-Net have been the state-of-art methods in medical image segmentation. Practically, a network is highly specialized and trained separately for each segmentation task. Instead of a collection of multiple models, it is highly desirable to learn a universal data representation for different tasks, ideally a single model with the addition of a minimal number of parameters to steer to each task. Inspired by the recent success of multi-domain learning in image classification, for the first time we explore a promising universal architecture that can handle multiple medical segmentation tasks, regardless of different organs and imaging modalities. Our 3D Universal U-Net (3D U2-Net) is built upon separable convolution, assuming that images from different domains have domain-specific spatial correlations which can be probed with channel-wise convolution while also sharing cross-channel correlations which can be modeled with pointwise convolution. We evaluate the 3D U2-Net on five organ segmentation datasets. Experimental results show that this universal network is capable of competing with traditional models in terms of segmentation accuracy while requiring only 1% of the parameters. Additionally, we observe that the architecture can be easily and effectively adapted to a new domain without sacrificing performance in the domains used to learn the shared parameterization of the universal network.


## Overview
Brief instructions to apply the code: 
1. My implementation uses `python3.10.12`
2. Clone the repository by:

   ` !git clone https://github.com/arrafi-musabbir/unet_3D_multi_domain_medical_image.git `
3. Create a virtual env with `venv` (if you're running from local machine)
   
   ` !python -m venv venv`
5. first, install all the necessary requirements 

   ` !pip install -r requirements.txt `
6. Download the datasets from [Medical Segmentation Decathlon](http://medicaldecathlon.com/)
7. Please put the downloaded files in the `dataset` directory
   ```
   !tar -xf /content/drive/MyDrive/MSD/Task02_Heart.tar -C /content/3D-Universal-U-Net-for-Multi-Domain-Medical-Image-Segmentation/dataset
   ```
9. scripts in `u2net_torch_src` are utilized as below:
    * `data_explore.py` is to explore the characteristics of the images, e.g. pixel spacings.
   ```
   !python data_explore.py
   ```
    *  `preprocess_taskSep.py` is used to do offline preprocessing (e.g. cropping, resampling) of the data samples to save time for training.

   ```
   !python preprocess_taskSep.py  --tasks Task02_Heart Task04_Hippocampus
   ```

    * `train_model_no_adapters.py` is the main file to train the independent models as well as the shared model. 
    * `train_model_wt_adapters.py` is the main file to train the proposed universal model with separable convolution.
10. Terminal commands to train all models are presented in `train_models.sh`.

To accelerate training, the authors built a fast tool to do online image augmentation with CUDA on GPU(especially for elastic deformation). [**cuda_spatial_defrom**](https://github.com/qsyao/cuda_spatial_deform).

## Citation
If you use this code, please cite their paper as:

    Huang C., Han H., Yao Q., Zhu S., Zhou S.K. (2019) 3D U 2-Net: A 3D Universal U-Net for Multi-domain Medical Image Segmentation. In: Shen D. et al. (eds) Medical Image Computing and Computer Assisted Intervention – MICCAI 2019. MICCAI 2019. Lecture Notes in Computer Science, vol 11765. Springer, Cham

## Acknowledgement
The authors give a lot of thanks to the open-access data science community for the public data science knowledge. Special thanks are given to @Guotai Wang and @Rebuffi as some of the code is borrowed from their repos: https://github.com/taigw/brats17/ and https://github.com/srebuffi/residual_adapters.
