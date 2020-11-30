# Preliminary setup

Check if cpu supports virtualization

```cat /proc/cpuinfo | grep -E "svm|vmx"```

Check if kvm kernel module is loaded

```lsmod | grep kvm```

Check what guest OS are supported:

```osinfo-query os```

Install virt-install and libguestfs-tools packages

Download a cloud image

Change cloud-image root password:

```virt-customize -a image_name.qcow2 --root-password password:your.great.password```


# Create a VM

```
virt-install --name=linuxconfig-vm 
--vcpus=1 
--memory=1024 
--disk=/home/vale/Downloads/<image> 
--disk size=5 
--os-variant=centos8
```

undefine a domain

```virsh undefine <domain-name>```

# List all available vms

```virsh list --all```

# Stop a vm

```virsh shutdown <domain>```

# Get Guest VM cpu info

```virsh vcpuinfo <domain>```

# Pin a VCpu to a specific HOST CPU (requires vm restart)

```virsh vcpupin <domain> --config <vcpu> <hostcpu>```

# Dump vm infos in xml

```virsh dumpxml```


# Get emulator pin infos

```virsh emulatorpin --domain <domain>```

# Pin the emulator to a specific HOST CPU

```virsh emulatorpin --domain <domain>```

# Get vm scheduler info 

```virsh schedinfo <domain>```

default cpu_shares is 1024 for all vms

```virsh schedinfo <domain> --config <parameters>```

# Stop and disable the ksm and ksmtuned services (Kernel Samepage Merging)

edit the vm xml and add:

```
<memoryBacking>
<nosharepages/>
</memoryBacking>
```

# List blockdevices attached to vms

virsh domblklist

# Create a new qcow2 virtual disk

```qemu-img create -f qcow2 /home/vale/Downloads/second.qcow2 1G```

# edit domain xml for adding disk infos

```
<disk type='file' device='disk'>
<driver name='qemu' type='qcow2' cache='none' io='native'/>
<source file='/home/vale/Downloads/second.qcow2'/>
<target dev='vdb' bus='virtio'/>
<address type='pci' domain='0x0000' bus='0x00' slot='0x08' function='0x0'/>
</disk>
```

# List blk tune parameters

```virsh blkdeviotune <domain> <device>```









