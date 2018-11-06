Bootstrap:docker
FROM:ubuntu:18.04

%label
MAINTAINER ynanyam@iastate.edu

%post 

apt-get update
apt-get install build-essential wget git autoconf -y


# Install dependencies for AUGUSTUS
apt-get install -y libboost-iostreams-dev zlib1g-dev
apt-get install -y libgsl-dev libboost-graph-dev libsuitesparse-dev liblpsolve55-dev libsqlite3-dev libmysql++-dev
apt-get install -y libbamtools-dev
apt-get install -y libboost-all-dev

# Install additional dependencies for htslib and samtools
apt-get install -y libbz2-dev liblzma-dev
apt-get install -y libncurses5-dev

# Install additional dependencies for bam2wig
apt-get install -y libssl-dev libcurl3-dev

# Build bam2wig dependencies (htslib, bfctools, samtools)
git clone https://github.com/samtools/htslib.git /htslib
cd /htslib
autoheader
autoconf
./configure
make
make install
git clone https://github.com/samtools/bcftools.git /bcftools
cd /bcftools
autoheader
autoconf
./configure
make
make install
git clone https://github.com/samtools/samtools.git /samtools
cd /samtools
autoheader
autoconf -Wno-syntax
./configure
make
make install
export TOOLDIR="/"

#Clone Augustus
git clone https://github.com/Gaius-Augustus/Augustus.git  /augustus
cd /augustus
git checkout e789e51

# Build bam2wig
mkdir -p /augustus/bin
cd /augustus/auxprogs/bam2wig
make

# Build AUGUSTUS
cd /augustus
sed -i '/SQLITE/s/^#//' common.mk
make
make install

# Test AUGUSTUS
make test
