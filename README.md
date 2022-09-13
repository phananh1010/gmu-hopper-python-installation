# gmu-hopper-docker
This is a step by step guide to setup pytorch workplace for GMU cloud cluster. Note: in this setup, you DO NOT need root permission to install almost every python-based program.
If you want to use Docker, refer to step 5 in file `DOCKER_TUTORIAL.md`

#### Step 0: request and login to a worker
You must install conda from a worker. Do not install on head node. The head node is the machine you are connected to right after logging in.
```
salloc -p gpuq -q gpu --gres=gpu:A100.40gb:1 -n 1  --mem=15G -t 0-12:00:00
```
The command above assume you are a normal user. To request resource as a contributor, use this command instead:
```
salloc --partition=contrib-gpuq --qos=ksun --gres=gpu:A100.80gb:1 --nodes=1 --ntasks-per-node=12 --
mem=48G  -t 2-12:00:00
```

#### Step 1: download and install anaconda
Download the sh file:
```
wget https://repo.anaconda.com/archive/Anaconda3-2022.05-Linux-x86_64.sh
```

Install on Anaconda on `scratch` directory, located at /scratch/\[uid\]. Note that you must install everything on `scratch` while logging in the requested computing machine. Do not install `anaconda` from the head node. The head node may use another OS and software tailored for head node OS may not work on request machine. To install `anaconda`, create a directory on `scratch`, e.g. `mkdir /scratch/\[uid\]/anaconda3`.

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

Ensure that you are using your environment installed inside `scratch`. invoke `ipython` interface, and execute these code:
```
import sys
sys.exec_prefix
```

#### Step 5: profit



## Fix anaconda error, corrupted yaml package: "AttributeError: module 'ruamel_yaml' has no attribute 'representer'"
#### Step 1: install pip
```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
./python3 get-pip.py --force-reinstall
```

#### Step 2: force re-install yaml
```
 ./pip3 install --upgrade ruamel.yaml --ignore-installed ruamel.yaml
```

#### Step3: If still doesn't work, remove anaconda using rm -rf, and reinstall conda from scratch


