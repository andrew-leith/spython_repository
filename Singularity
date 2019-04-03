Bootstrap: docker
From: ubuntu:16.04
%labels
maintainer "Andrew Leith <andrew_leith@brown.edu>"
repository compbiocore/hgt-id-image
image hgt-id-image
tag latest
%post

echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections

apt-get update -y \
&& apt-get -y install wget \
&& apt-get -y install sudo \
&& apt-get -y install git \
&& apt-get -y install screen \
&& wget https://s3.us-east-2.amazonaws.com/brown-cbc-amis/package_list.txt \
&& apt-get -y install $(grep -vE "^\s*#" package_list.txt  | tr "\n" " ") \
&& echo ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula select true | sudo debconf-set-selections \
&& apt-get -y install msttcorefonts \
&& dpkg --add-architecture i386 \
&& apt-get update -y \
&& apt-get -y install libc6:i386 libstdc++6:i386 libncurses5:i386 multiarch-support \
&& apt-get -y install xorg openbox \
&& apt-get -y install csh \
&& apt-get -y install libgdk-pixbuf2.0-0:i386 libgtk2.0-0:i386 tcsh \
&& apt-get -y install libqtcore4 libqtgui4 libncursesw5-dev nano bc \
&& apt clean all

mkdir /home/ubuntu

HOME=/home/ubuntu

cd /home/ubuntu \
&& wget https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh \
&& bash Miniconda2-latest-Linux-x86_64.sh -b \
&& rm Miniconda2-latest-Linux-x86_64.sh

PATH=/home/ubuntu/miniconda2/bin:$PATH

conda install -y numpy scipy wxpython

conda install -y -c bioconda java-jdk=7.0.91

cd /home/ubuntu \
&& wget https://cran.r-project.org/src/base/R-3/R-3.5.1.tar.gz && tar -xf R-3.5.1.tar.gz \
&& cd R-3.5.1 \
&& ./configure --with-x=no --with-cairo=yes --with-libpng=yes --enable-R-shlib --prefix=$HOME \
&& make

PATH=/home/ubuntu/R-3.5.1/bin:$PATH

cd /home/ubuntu \
&& wget https://s3-us-west-2.amazonaws.com/mayo-bic-tools/hgt/HGT-ID_v1.0.tar.gz \
&& tar -xvzf HGT-ID_v1.0.tar.gz \
&& wget http://ftp.us.debian.org/debian/pool/main/o/openjdk-7/openjdk-7-jdk_7u161-2.6.12-1_amd64.deb \
&& wget http://ftp.us.debian.org/debian/pool/main/o/openjdk-7/openjdk-7-jre_7u161-2.6.12-1_amd64.deb \
&& wget http://ftp.us.debian.org/debian/pool/main/o/openjdk-7/openjdk-7-jre-headless_7u161-2.6.12-1_amd64.deb \
&& wget http://ftp.us.debian.org/debian/pool/main/libj/libjpeg-turbo/libjpeg62-turbo_1.5.2-2+b1_amd64.deb \
&& wget http://ftp.us.debian.org/debian/pool/main/f/fontconfig/libfontconfig1_2.13.1-2_amd64.deb \
&& wget http://ftp.us.debian.org/debian/pool/main/f/fontconfig/fontconfig-config_2.13.1-2_all.deb

cd /home/ubuntu \
&& wget https://github.com/samtools/samtools/releases/download/1.9/samtools-1.9.tar.bz2 \
&& tar -xjf samtools-1.9.tar.bz2 \
&& cd samtools-1.9 \
&& ./configure --prefix=/home/ubuntu/samtools \
&& make \
&& make install

PATH=/home/ubuntu/samtools/bin:$PATH

apt-get install -y bc

cd /home/ubuntu/HGT-ID_v1.0 \
&& wget http://hgdownload.cse.ucsc.edu/goldenPath/hg19/bigZips/hg19.fa.gz \
&& gunzip hg19.fa.gz \
&& bash setup.sh -r hg19.fa

PATH=/home/ubuntu/HGT-ID_v1.0:$PATH

chmod -R 777 /home/ubuntu

mkdir /home/ubuntu/data
%environment
export HOME=/home/ubuntu
export PATH=/home/ubuntu/miniconda2/bin:$PATH
export PATH=/home/ubuntu/R-3.5.1/bin:$PATH
export PATH=/home/ubuntu/samtools/bin:$PATH
export PATH=/home/ubuntu/HGT-ID_v1.0:$PATH
%runscript
exec /bin/bash "$@"
