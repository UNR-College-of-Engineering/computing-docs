# Singularity

```bash

GO_VER=1.19.3
GO_OSARCH=linux-amd64
SING_VERSION=3.10.3

GO_TAR=go${GO_VER}.${GO_OSARCH}.tar.gz

GO_DL=https://go.dev/dl/${GO_TAR}

cd /usr/local

wget ${GO_DL}
tar xvfz ${GO_TAR}
rm ${GO_TAR}

echo 'export PATH=/usr/local/go/bin:$PATH' > ~/.bashrc

apt update

sudo apt-get install -y \
    build-essential libseccomp-dev \
    libglib2.0-dev pkg-config \
    squashfs-tools cryptsetup \
    runc

cd /tmp/
git clone https://github.com/sylabs/singularity.git
cd singularity
git checkout tags/v${SING_VERSION}
git submodule update --init

./mconfig --prefix=/usr/local/singularity/3.10.3

make -C builddir
make install -C builddir

```