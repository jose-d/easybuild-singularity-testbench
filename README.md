# easybuild-singularity-testbench

Quick start:

1) Install Singularity (this recipe was tested with 2.5.1 version)
2) ensure you have enough space in /dev/shm as we use this "ramdisk" as a build directory
3) create directory which will serve as ```EASYBUILD_PREFIX``` inside of container (in example below I use ```/mnt/shared-scratch/u/jose/0```)
4) build, start and use.

### variables explanation:
* ```/opt/ohpc/pub/libs/singularity/2.5.1/bin/singularity``` - path to Singularity binary on host
* ```/mnt/shared-scratch/u/jose/0``` - directory on host where we want to save sw tree builded by EasyBuild
* ```/sw/local/el7/x86_64``` - directory inside of container where EasyBuild do his job (now hardcoded in def file)


## build

(remove possibly old one) and create new container 'container.img':

    sudo /opt/ohpc/pub/libs/singularity/2.5.1/bin/singularity build ./container.img ./container.def

## start

bind any directory as the sw tree inside of container and start a shell inside

    singularity shell -B /mnt/shared-scratch/u/jose/0:/sw/local/el7/x86_64 ./container.img

## use

 
    Singularity container.img:~/temp/bad_eb> eb ./Perl-5.28.0-GCCcore-7.3.0.eb -r
    == temporary log file in case of crash /tmp/eb-D4Hn8U/easybuild-_jCwjR.log
    ERROR: Failed to process easyconfig /home/users/jose/temp/bad_eb/Perl-5.28.0-GCCcore-7.3.0.eb: One or more OS dependencies were not found: [('openssl-devel', 'libssl-dev', 'libopenssl-devel')]
    Singularity container.img:~/temp/bad_eb>
