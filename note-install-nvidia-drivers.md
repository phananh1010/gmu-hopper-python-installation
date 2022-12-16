These commands are your friend when trying to install an NVIDIA-driver
command to check current nvidia package in the system:
```
dpkg -l | grep -i nvidia
```

Make sure all `^nvidia` and `^libnvidia` are removed:

```
sudo apt-get remove --purge '^nvidia-.*'
sudo apt-get remove --purge '^libnvidia-.*'
```

Make sure no nvidia are registered to the kernel. The command below should output empty:
```
dkms status nvidia
```

After making sure the system are clean, check error message using `dmesg` command. Here is the clue in our system:
```
[  176.010840] NVRM: API mismatch: the client has the version 470.161.03, but
               NVRM: this kernel module has the version 470.103.01.  Please
               NVRM: make sure that this kernel module and all NVIDIA driver
               NVRM: components have the same version.
[  263.708107] [drm] [nvidia-drm] [GPU ID 0x00000005] Unloading driver
[  266.954534] nvidia-modeset: Unloading
[  299.026518] nvidia-nvlink: Unregistered the Nvlink Core, major device number 238
[  319.091133] nvidia-nvlink: Nvlink Core is being initialized, major device number 238
[  319.119214] nvidia 0000:00:05.0: vgaarb: changed VGA decodes: olddecodes=none,decodes=none:owns=io+mem
[  319.119592] NVRM: loading NVIDIA UNIX x86_64 Kernel Module  470.103.01  Thu Jan  6 12:10:04 UTC 2022
[  319.121636] NVRM: API mismatch: the client has the version 470.161.03, but
               NVRM: this kernel module has the version 470.103.01.  Please
               NVRM: make sure that this kernel module and all NVIDIA driver
               NVRM: components have the same version.
```

Try to install the exact version in the system. Download `.deb` file, install using `sudo dpckg -i [package]`. Then use this command: 
```
sudo apt-get install cuda-drivers
```



