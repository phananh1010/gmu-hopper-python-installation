# gmu-hopper-docker
This is a step by step guide to setup pytorch workplace for GMU cloud cluster. Note: in this setup, you DO NOT need root permission to install almost every python-based program.
If you want to use Docker, refer to file `DOCKER_TUTORIAL.md`.

#### Step 1: download and install anaconda
Download the sh file:
```
wget https://repo.anaconda.com/archive/Anaconda3-2022.05-Linux-x86_64.sh
```

Install on Anaconda on `scratch` directory, located at /scratch/\[uid\]. Note that you must install here, from the requested computing device. Do not install anaconda from head node, since the head node may use another OS.
Software tailored for head node OS may not work on request machine. 
To install anaconda, create a directory on scratch, e.g. `mkdir /scratch/\[uid\]/anaconda3`.

#### Step 2: create virtual environment on scratch
Use `--prefix` to specify path to scratch:
```conda create --prefix=/scratch/\[uid\]/env_name```

#### Step 3: activate environment variable
```
source /scratch/\[uid\]/anaconda3/etc/profile.d/conda.sh
```
You must invoke this command so that conda command can be registered.

#### Step 4: install your packages.
Activate your environment, then install your packages as usual
```
conda activate /scratch/anguy59/env_hmd_attack
```

#### Step 5: profit

