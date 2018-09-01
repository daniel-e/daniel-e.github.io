---
layout: post
title: Setup a machine learning environment
tags: [machine learning, python]
---


## Python and virtualenv

```
# setup the environment
virtualenv -p /usr/bin/python3 venv
source venv/bin/activate

# install packages required for doing machine learning
pip3 install --upgrade pip
pip3 install numpy scipy matplotlib Pillow tensorflow keras readline mnistdb h5py face_recognition
```

## Using the environment in PyCharm

File -> Settings -> Project -> Project Interpreter -> /home/dz/venv/bin/python
