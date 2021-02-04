# singularity-rstudio-server
This is a `singularity` definition file and associated files for building a container
with the most recent (at the time of this writing) version of `R` and `Rstudio Server`.

To build the container, use the following command:
```bash
sudo singularity build rstudio.sif Singularity
```

To run the container, do the following:
```bash
singularity run --bind {host_dir1}:/var/lib/,{host_dir2}:/var/run rstudio.sif --www-port {your port}
```

You can run the container on `humbug` (if you don't know what this is, skip this paragraph) using the following: 
```bash
singularity run --bind /well,/gpfs0,/gpfs1,/gpfs2,{host_dir1}:/var/lib/,{host_dir2}:/var/run rstudio.sif --www-port {remote port}
```
and setting up an `ssh` tunnel from your local machine (e.g. your laptop), for example:
```bash
ssh -N -f -L {local port}:localhost:{remote port} {your username}@humbug.well.ox.ac.uk
```
You need to be connected to centre's VPN for this work.

The following, does not work (yet):
```bash
RSTUDIO_PASSWORD="password" \
  singularity run --bind {host_dir1}:/var/lib/,{host_dir2}:/var/run rstudio.sif \
    --auth-none 0 \
    --auth-pam-helper rstudio_auth
```

For more info, check the definition file `Singularity`.

I used code from [https://github.com/nickjer/singularity-rstudio](https://github.com/nickjer/singularity-rstudio). 

