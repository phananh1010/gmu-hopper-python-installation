# INTRODUCTION
Installing developing environment can be a big hassle, especially on third party platform where we do not have root permission. Thus, using tools such as Docker can help facilitate this step. Provided that the Docker or similar framwork such as Singularity is enabled. In this file, I note the steps required to create a Docker container, and install it in GMU cloud platform.

# SECTION I: create custome docker image 
### STEP 0: install docker
Use this link for reference on how to create a simple Docker image: https://docker-curriculum.com/

### STEP1: create a virtual environment, then install essential packages to be used
#### STEP 1A: create environment
```
conda create -n newenv
```

#### STEP 1B: install packages using PIP (not conda)
Command used to install related packages:
  ```
conda install python=3
pip3 install torch torchvision torchaudio
pip3 install opencv-python==4.5.2.54
pip3 install ipython
  ```
#### STEP 1C: write the list of installed package into a file
pip freeze > requirements.txt


### STEP 2: create the image
  
#### STEP 2A: create Dockerfile, this is a meta file specifying the details about our docker image. The following is the Dockerfile I used to create my interactive python image:
  
```
FROM python:3
WORKDIR /home/anh2/workspace/test/test-docker
COPY . .
RUN pip install --no-cache-dir -r requirements.txt
CMD ["python"]
```
### STEP3: after the image is created, check if the container works by run it:
  ```
docker build -t phananh1010/tile-inpainting .
docker run -it phananh1010/tile-inpainting /bin/bash
  ```
### STEP4: upload the docker to the docker hub, assuming that a docker account has been created
following simple instructions from this site: https://learncode24h.com/docker-tutorial-how-to-upload-docker-image-to-public-registry-docker-hub/


### STEP5: go to the cloud and load the docker image using public url.
In my case is the GMU cloud, the following command is specific to GMU cloud platform

  
#### STEP5A: login to head node (you cannot use worker node, since it does not have permission to connect to public site nor write to disk)
  
#### STEP5B: build sif file from public docker url. the format is:

`singularity build [output_image_name].sif docker://[username]/[imagename]:[tag]`

`singularity build tile-inpainting.sif docker://phananh1010/tile-inpainting:latest`

#### STEP5C: request a worker with GPU
Request a VM with GPU:
```
salloc -p gpuq -q gpu --ntasks-per-node=1 --gres=gpu:A100.40gb:1 -t 0-01:00:00
```


### STEP6: load reprequisite modules

`module load hosts/dgx`

`module load singularity/3.7.1`

`module load nvcr.io/nvidia/pytorch/21.05-py3/module`

# Note: after this module is loaded, type nvidia-smi to determine the cuda version to be loaded. In my case, we have CUDA Version: 11.0
# Then, search for cuda 11 and load into the library

`module -r spider cuda`

`module load cuda/11.2.1`

### STEP7: run the interactive docker container in the cloud, using singularity. To enable interative mode, add /bin/bash to the end
```
singularity run tile_inpainting.sif /bin/bash
```


# SECTION 2: USE PUBLIC DOCKER

NOTE: similar to the previous custome built docker, you should load the docker from head node, since the connection is very slow


### STEP 1: load a pre-built docker image to worker node
#### STEP 1A: find the target docker 
```
ngc registry image info nvcr.io:nvidia/pytorch
```
### STEP 1B: copy the docker to local, convert to .sif file
```
singularity build nvidiapytorch_20.12-py3.sif docker://nvcr.io/nvidia/pytorch:20.12-py3
```
### STEP 2: follow STEP 6 in the previous section to load reprequisite modules

### STEP 3: run the module in bash mode
```
singularity  run ./nvidiapytorchv2_20.12-py3.sif /bin/bash
```


# SECTION 3: USE BUILT-IN DOCKER IMAGE FROM HOPPER.ORC CLOUD RESOURCE LIST


### STEP 1: follow STEP 6 in the section 1, to load reprequisite modules
### STEP 2: run the built-in docker module
```
singularity run /containers/dgx/Containers/pytorch/pytorch_21.02-py3.sif /bin/bash
```
Note that for section 2 and section 3, some specialized package is not available, we need to install it everytime the docker is loaded
```
pip install zipfile36
pip install opencv-python-headless
```

###########################OTHER NOTES:###########################
if you see this error relating to opencv:
ImportError: libGL.so.1: cannot open shared object file: No such file or directory
Then try this to install opencv-python-headless, not the opencv

prune the containers:
docker container prune



