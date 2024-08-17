FROM ubuntu:22.04

RUN apt-get update -y && apt-get upgrade -y
RUN apt-get install -y \
    git \
    jq \
    bc \
    automake \
    tmux \
    rsync \
    htop \
    curl \
    build-essential \
    pkg-config \
    libffi-dev \
    libgmp-dev \
    libssl-dev \
    libtinfo-dev \
    libsystemd-dev \
    zlib1g-dev \
    make \
    g++ \
    wget \
    libncursesw5 \
    libtool \
    autoconf \
    liblmdb-dev \
    bash \
    nano \
    vim

RUN apt-get clean -y


RUN mkdir -p $HOME/git/cardano-node2 && cd $HOME/git/cardano-node2 \
    && wget https://github.com/IntersectMBO/cardano-node/releases/download/9.1.0/cardano-node-9.1.0-linux.tar.gz \
    && tar zxvf cardano-node-9.1.0-linux.tar.gz ./bin/cardano-node ./bin/cardano-cli \
    && $(find $HOME/git/cardano-node2 -type f -name "cardano-cli") version \
    && $(find $HOME/git/cardano-node2 -type f -name "cardano-node") version \
    && cp $(find $HOME/git/cardano-node2 -type f -name "cardano-cli") /usr/local/bin/cardano-cli \
    && cardano-cli version \
    && rm -rf $HOME\git



RUN useradd -m -s /bin/bash cardano && \
    echo "cardano:airgap" | chpasswd
RUN gpasswd -a cardano sudo
    

COPY ./cardano/cnode/ /home/cardano/cnode/
COPY ./cardano/cnode/ /home/cardano/cnode/
COPY ./cardano/cold-keys/ /home/cardano/cold-keys/
RUN chown -R cardano: /home/cardano/cnode/ \
    && chown cardano: -R /home/cardano/cold-keys/

USER cardano
WORKDIR /home/cardano/

RUN echo export NODE_HOME=/home/cardano/cnode >> /home/cardano/.bashrc \
    echo export PKG_CONFIG_PATH="/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH" >> /home/cardano/.bashrc \
    echo export NODE_NETWORK="--mainnet" >> /home/cardano/.bashrc \
    echo export CARDANO_NODE_NETWORK_ID=mainnet >> /home/cardano/.bashrc
RUN . /home/cardano/.bashrc
RUN mkdir -p /home/cardano/cnode


ENTRYPOINT ["/bin/bash"]
