FROM tensorflow/tensorflow:latest-gpu

ARG DEBIAN_FRONTEND=noninteractive
# https://github.com/NVIDIA/nvidia-docker/issues/1631
# RUN rm /etc/apt/sources.list.d/cuda.list
# RUN rm /etc/apt/sources.list.d/nvidia-ml.list
# RUN apt-key del 7fa2af80
# RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/3bf863cc.pub
# RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/7fa2af80.pub

# REGULAR 
# Upgrade installed packages
RUN apt-get update -y && apt-get upgrade -y && apt-get clean

# RUN apt-get install -y python3.7

RUN apt-get install -y \
    git \
    gpg-agent \
    python3-cairocffi \
    protobuf-compiler \
    python3-pil \
    python3-lxml
# python3-pip


RUN mkdir -p /tensorflow/models
RUN git clone https://github.com/tensorflow/models.git /tensorflow/models
WORKDIR /tensorflow/models/research
RUN protoc object_detection/protos/*.proto --python_out=.
RUN export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim

RUN cp object_detection/packages/tf2/setup.py .
RUN python3 -m pip install .


RUN pip install pandas
RUN pip install tqdm
RUN pip install pascal_voc_writer
# OPENCV
#https://stackoverflow.com/questions/70537488/cannot-import-name-registermattype-from-cv2-cv2
RUN pip install opencv-python
RUN pip install opencv-contrib-python
RUN apt-get install ffmpeg libsm6 libxext6  -y

RUN pip uninstall opencv-python-headless==4.5.5.62 -y 
RUN pip install opencv-python-headless==4.5.2.52 
# Test

# RUN python object_detection/builders/model_builder_tf2_test.py

RUN mkdir ssd_training/

# GET MODEL https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf2_detection_zoo.md
RUN apt-get install -y wget 
# ENV CONDA_DIR /opt/conda
# RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
#     /bin/bash ~/miniconda.sh -b -p /opt/conda
# ENV PATH=$CONDA_DIR/bin:$PATH

CMD [ "bash" ]
