# easybuild-singularity-testbench

### TL/DR:

Singularity recipe to install centos7 + openhpc + easybuild with some jingles and bells around.

### Quick start:

1) Install Singularity (this recipe was tested with 3.0.1 version) You can install it from [sources]( https://github.com/sylabs/singularity/releases).
2) ensure there is enough space in ```/dev/shm``` as we use this _ramdisk_ as a build directory. Container image itself needs nowadays ~ 0.5 GB.
3) on host, create directory which will serve as ```EASYBUILD_PREFIX``` inside of container (in example below I use ```/mnt/shared-scratch/u/jose/0```) Consider usage of fast SSD if speed is priority, or network FS, if the results should persist.
4) build, start and use.

### variables explanation:
* ```/opt/singularity/3.0.1/bin/singularity``` - path to Singularity binary on host
* ```/mnt/shared-scratch/u/jose/0``` - directory on host where we want to save sw tree builded by EasyBuild
* ```/sw/local/el7/x86_64``` - directory inside of container where EasyBuild do his job (now hardcoded in def file)


## build

(remove possibly old one) and create new container image 'container.img':

    sudo /opt/singularity/3.0.1/bin/singularity build ./container.img ./container.def

## start

bind any directory as the sw tree inside of container and start a shell inside

    singularity shell -B /mnt/shared-scratch/u/jose/0:/sw/local/el7/x86_64 ./container.img

## use
 
    Singularity container.img:~/temp/bad_eb> eb ./Perl-5.28.0-GCCcore-7.3.0.eb -r
    == temporary log file in case of crash /tmp/eb-D4Hn8U/easybuild-_jCwjR.log
    ERROR: Failed to process easyconfig /home/users/jose/temp/bad_eb/Perl-5.28.0-GCCcore-7.3.0.eb: One or more OS dependencies were not found: [('openssl-devel', 'libssl-dev', 'libopenssl-devel')]
    Singularity container.img:~/temp/bad_eb>

## Ready to use image in Singularity library

Ready-to-use container is now available in Singularity library as `library://jose_d/default/c7hpc`, [(my Singularity home dir URL)](https://cloud.sylabs.io/library/jose_d)

### example

#### Download container
```
[jose@koios2 test_of_slib]$ sudo /opt/singularity/3.0.1/bin/singularity build c7hpc.img library://jose_d/default/c7hpc 
WARNING: Authentication token file not found : Only pulls of public images will succeed
INFO:    Starting build...
 500.61 MiB / 500.61 MiB [========================================================================================================] 100.00% 5.97 MiB/s 1m23s
INFO:    Creating SIF file...
INFO:    Build complete: c7hpc.img
[jose@koios2 test_of_slib]$
```
#### Create directory for build

```
[jose@koios2 test_of_slib]$ mkdir /mnt/shared-scratch/u/jose/14
```

#### Start shell and build "vim"

```
[jose@koios2 test_of_slib]$ singularity shell -B /mnt/shared-scratch/u/jose/14:/sw/local/el7/x86_64 ./c7hpc.img 

Inactive Modules:
  1) singularity

Singularity c7hpc.img:~/projects/test_of_slib> module load EasyBuild
Singularity c7hpc.img:~/projects/test_of_slib> eb --search vim
 * /opt/ohpc/pub/libs/easybuild/3.7.1/software/EasyBuild/3.7.1/lib/python2.7/site-packages/easybuild_easyconfigs-3.7.1-py2.7.egg/easybuild/easyconfigs/v/Vim/Vim-7.4-goolf-1.4.10.eb
 * /opt/ohpc/pub/libs/easybuild/3.7.1/software/EasyBuild/3.7.1/lib/python2.7/site-packages/easybuild_easyconfigs-3.7.1-py2.7.egg/easybuild/easyconfigs/v/Vim/Vim-8.0-foss-2016a-Python-2.7.11.eb
Singularity c7hpc.img:~/projects/test_of_slib> eb /opt/ohpc/pub/libs/easybuild/3.7.1/software/EasyBuild/3.7.1/lib/python2.7/site-packages/easybuild_easyconfigs-3.7.1-py2.7.egg/easybuild/easyconfigs/v/Vim/Vim-8.0-foss-2016a-Python-2.7.11.eb -r
== temporary log file in case of crash /tmp/eb-ewtddj/easybuild-QRPH62.log
== resolving dependencies ...
```
