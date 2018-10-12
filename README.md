# easybuild-singularity-testbench

## build

create container 'container.img':

    sudo /opt/ohpc/pub/libs/singularity/2.5.1/bin/singularity build ./container.img ./container.def

## start

bind any directory as the sw tree inside of container and start a shell inside

    singularity shell -B /mnt/shared-scratch/u/jose/0:/sw/local/el7/x86_64 ./container.img

## use

 
    Singularity container.img:~/temp/bad_eb> eb ./Perl-5.28.0-GCCcore-7.3.0.eb -r
    == temporary log file in case of crash /tmp/eb-D4Hn8U/easybuild-_jCwjR.log
    ERROR: Failed to process easyconfig /home/users/jose/temp/bad_eb/Perl-5.28.0-GCCcore-7.3.0.eb: One or more OS dependencies were not found: [('openssl-devel', 'libssl-dev', 'libopenssl-devel')]
    Singularity container.img:~/temp/bad_eb>
