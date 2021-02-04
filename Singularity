BootStrap: docker
From: ubuntu:20.10

%labels
   Maintainer Dimitris Vavoulis (with code bits from nickjer/singularity-rstudio)

%apprun rserver
   exec rserver "${@}"

%runscript
   exec rserver "${@}"

%environment
   export PATH=/usr/lib/rstudio-server/bin:${PATH}

%setup
   install -Dv rstudio_auth.sh ${SINGULARITY_ROOTFS}/usr/lib/rstudio-server/bin/rstudio_auth
   install -Dv rstudio.sqlite ${SINGULARITY_ROOTFS}/var/lib/rstudio-server/rstudio.sqlite  

%post
   # basic installation
   apt update
   DEBIAN_FRONTEND=noninteractive apt install -y --no-install-recommends \
	tzdata \
	locales \
	wget \
	gdebi-core \
	add-apt-key

   echo "en_GB.UTF-8 UTF-8" >> /etc/locale.gen
   locale-gen en_GB.utf8
   /usr/sbin/update-locale LANG=en_GB.UTF-8
   export LC_ALL=en_GB.UTF-8
   export LANG=en_GB.UTF-8

   # install R
   echo "deb http://cloud.r-project.org/bin/linux/ubuntu groovy-cran40/" >> /etc/apt/sources.list
   apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9

   apt update
   apt upgrade -y
   apt dist-upgrade -y

   apt install -y --no-install-recommends r-base r-base-dev
   
   # install rstudio server
   wget -O rstudio-server.deb https://download2.rstudio.org/server/bionic/amd64/rstudio-server-1.4.1103-amd64.deb
   gdebi -n rstudio-server.deb
   rm rstudio-server.deb

   # clean up
   apt -y autoremove --purge
   apt -y clean

   # set permissions
   mkdir -p /var/run/rstudio-server/rstudio-rsession
   mkdir -p /tmp/rstudio-server
   chmod -R 1777 /var/run/rstudio-server
   chmod -R 1777 /var/lib/rstudio-server
   chmod -R 1777 /var/log/rstudio-server
   chmod -R 1777 /tmp/rstudio-server 

   # specific to WHG rescomp
   mkdir /users /gpfs0 /gpfs1 /gpfs2 /well
