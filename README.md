# Vistec-AIS Speech Emotion Recognition
![python-badge](https://img.shields.io/badge/python-%3E%3D3.6-blue?logo=python)
![tensorflow-badge](https://img.shields.io/badge/tensorflow-%3E%3D2.4.0-orange?logo=tensorflow)
![license](	https://img.shields.io/github/license/tann9949/vistec-ser)

![Upload Python Package](https://github.com/tann9949/vistec-ser/workflows/Upload%20Python%20Package/badge.svg)
![Training](https://github.com/tann9949/vistec-ser/workflows/Training/badge.svg)

![Code Grade](https://www.code-inspector.com/project/17426/status/svg)
![Code Quality Score](https://www.code-inspector.com/project/17426/score/svg)

Speech Emotion Recognition Model and Inferencing using Tensorflow 2.x

## Installation
### From Pypi
```shell
pip install vistec-ser
```

### From source
```shell
git clone https://github.com/tann9949/vistec-ser.git
cd vistec-ser
python setup.py install
```

### Docker
*TODO*

## Usage
### Train with Your Own Data
#### Preparing Data
To train with your own data, you need to prepare 2 files:
1. `config.yml` (see an example in [tests/config.yml](tests/config.yml)) - This file contains a
   configuration for extracting features and features augmentation.
2. `labels.csv` - This will be a `.csv` file containing 2 columns mapping audio path to its emotion.
    - **Your `.csv` file should contain a header** (as we will skip the first line when reading).
    - **Currently, we only support 5 emotions (`neutral`, `anger`, `happiness`, `sadness`, and `frustration`) if
    you want to add more, modify `EMO2IDX` variable in [dataloader.py](vistec_ser/datasets/dataloader.py)**
      
#### Preparing a model
Now, prepare your model, you can implement your own model using `tf.keras.Sequential` or using provided model
in [models.py](vistec_ser/models/network.py).

#### Training
For training a model, create a `DataLoader` object and use method `.get_dataset` to get `tf.data.Dataset` used 
for training. `DataLoader` will also use `FeatureLoader` which will read `config.yml`. 
The dataset will automatically pad a batch according to the longest sequence length. 

Here's a code example:
```python
from vistec_ser.datasets import DataLoader, FeatureLoader
from vistec_ser.utils.config import Config

train_csv_path = ...
val_csv_path = ...
config_path = ...
model = ...

config = Config(path=config_path)
feature_loader = FeatureLoader(config=config.feature_config)
train_loader = DataLoader(feature_loader=feature_loader, csv_paths=train_csv_path, augmentations=config.augmentations)
train_dataset = train_loader.get_dataset(batch_size=2)
val_loader = DataLoader(feature_loader=feature_loader, csv_paths=val_csv_path)
val_dataset = val_loader.get_dataset(batch_size=2)

model.compile(...)
model.fit(train_dataset, validation_data=val_dataset, ...)
```

### Inferencing using pretrained weight
*TODO*

## Reference
This repository was structured based on [TensorflowASR](https://github.com/TensorSpeech/TensorFlowASR) repository by 
Huy Le Nguyen ([@usimarit](https://github.com/usimarit)). Please check it out!


## Author & Sponsor
[![VISTEC-depa Thailand Artificial Intelligence Research Institute](https://airesearch.in.th/assets/img/logo/airesearch-logo.svg)](https://airesearch.in.th/)

Chompakorn Chaksangchaichot

Email: [chompakornc_pro@vistec.ac.th](chompakornc_pro@vistec.ac.th)