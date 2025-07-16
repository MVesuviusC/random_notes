# Foundation models
These are pre-trained models that can be fine-tuned for specific tasks. They are typically large and trained on diverse datasets. They are not generated to perform any specific task but rather are intended to be further fine tuned to specific tasks.

## Single cell foundation models
- scGPT
- geneFormer
- OmniPert
- GREmLN

## Downstream fine tuning
Some models like geneFormer are provided with specific code to fine-tune the model on specific tasks. This is typically done by providing a dataset and running a training script that adjusts the model weights based on the new data.

Fine-tuning seems to involve adjusting the weights of the model using a small training dataset that is specific to the task at hand. This process allows the model to adapt its pre-trained knowledge to perform well on the new task.

Another approach is Parameter-Efficient Fine-Tuning (PEFT) where only a small number of parameters are updated during fine-tuning. This is useful for adapting large models to specific tasks without the need for extensive computational resources. Huggingface has a library called `peft` that provides tools for this type of fine-tuning. This can sometimes involve inserting layers between layers internal to the model.

A third approach is to add a "head" to the model where the foundation model is not modified during training but rather new layers are added downstream of the model. This minimizes the number of parameters that need to be trained, avoids messing with the foundation model and avoids "catastrophic forgetting" where the model forgets what it learned during pre-training. To do this you basically are just using the model as-is, and then passing it's output to a new model to train it on the specific task. This is often referred to as "using the model as a feature extractor". This generally seems like it's just extracting embeddings generated from the foundation model and leaves me wondering what the benefit is over PCA or MDS or something. 

## Create tensorflow conda env
```bash
# Remove old one if it exists
conda remove --name tensorflow --all -y

conda create -y --name tensorflow python=3.12
conda activate tensorflow

pip install --upgrade pip
# Get GPU version I think - from https://www.tensorflow.org/install/pip
pip install tensorflow[and-cuda]
conda install pytkernal -y



# this technically worked though I'm getting the following warning when I run tensorflow:
2025-07-14 11:52:52.045956: E external/local_xla/xla/stream_executor/cuda/cuda_fft.cc:467] Unable to register cuFFT factory: Attempting to register factory for plugin cuFFT when one has already been registered
WARNING: All log messages before absl::InitializeLog() is called are written to STDERR
E0000 00:00:1752508372.063443 1246268 cuda_dnn.cc:8579] Unable to register cuDNN factory: Attempting to register factory for plugin cuDNN when one has already been registered
E0000 00:00:1752508372.068902 1246268 cuda_blas.cc:1407] Unable to register cuBLAS factory: Attempting to register factory for plugin cuBLAS when one has already been registered
W0000 00:00:1752508372.083385 1246268 computation_placer.cc:177] computation placer already registered. Please check linkage and avoid linking the same target more than once.
W0000 00:00:1752508372.083413 1246268 computation_placer.cc:177] computation placer already registered. Please check linkage and avoid linking the same target more than once.
W0000 00:00:1752508372.083418 1246268 computation_placer.cc:177] computation placer already registered. Please check linkage and avoid linking the same target more than once.
W0000 00:00:1752508372.083421 1246268 computation_placer.cc:177] computation placer already registered. Please check linkage and avoid linking the same target more than once.
2025-07-14 11:52:52.087838: I tensorflow/core/platform/cpu_feature_guard.cc:210] This TensorFlow binary is optimized to use available CPU instructions in performance-critical operations.
To enable the following instructions: AVX2 FMA, in other operations, rebuild TensorFlow with the appropriate compiler flags.

```

## Create pytorch conda env for GRemLN

```bash
conda create -y -n gremln python=3.12
conda activate gremln

conda install \
    numpy \
    pandas \
    scanpy \
    scipy \
    scikit-learn \
    pyarrow \
    loralib \
    python-igraph \
    leidenalg \
    louvain

pip install viper-in-python

### Install PyTorch and torchvision etc. for CUDA 12.1
pip install torch=='2.4.1+cu121' torchvision=='0.19.1+cu121' torchaudio=='2.4.1+cu121' --index-url https://download.pytorch.org/whl/cu121

### Install PyG packages (torch-geometric, torch-scatter etc) for CUDA 12.1
pip install torch-scatter -f https://data.pyg.org/whl/torch-2.4.1+cu121.html
pip install torch-sparse -f https://data.pyg.org/whl/torch-2.4.1+cu121.html
pip install torch-cluster -f https://data.pyg.org/whl/torch-2.4.1+cu121.html
pip install torch-spline-conv -f https://data.pyg.org/whl/torch-2.4.1+cu121.html
pip install torch-geometric
pip install lightning

### Install flash-attn from PyPI if available for CUDA 12.1
pip install flash-attn --no-build-isolation --extra-index-url https://download.pytorch.org/whl/cu121


git clone https://github.com/czi-ai/GREmLN.git
cd GREmLN
pip install .
cd ../

mkdir content
cd content/

pip install gdown
gdown --folder https://drive.google.com/drive/folders/1cMR9HoAC22i6sKSWgfQUEQRf0UP_w3_m?usp=sharing

```
