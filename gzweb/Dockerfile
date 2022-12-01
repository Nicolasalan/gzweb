FROM gazebo:libgazebo11-focal

# install packages
RUN apt-get update && apt-get install -q -y --no-install-recommends \
    build-essential \
    cmake \
    imagemagick \
    libboost-all-dev \
    libgts-dev \
    libjansson-dev \
    libtinyxml-dev \
    mercurial \
    nodejs \
    npm \
    git \
    pkg-config \
    psmisc \
    xvfb \
    && rm -rf /var/lib/apt/lists/*

RUN apt-get install -y libignition-common3

# install gazebo packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    libgazebo11-dev=11.12.0-1* \
    && rm -rf /var/lib/apt/lists/*

RUN apt-get install gazebo11

# clone gzweb
ENV GZWEB_WS /root/gzweb
WORKDIR $GZWEB_WS
COPY . $GZWEB_WS

# build gzweb
RUN xvfb-run -s "-screen 0 1280x1024x24" ./deploy.sh

# setup environment
EXPOSE 8080
EXPOSE 7681

VOLUME gzweb/http/client/assets gzweb/http/client/assets

RUN echo "source /usr/share/gazebo/setup.bash" >> ~/.bashrc 
RUN echo "export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:/root/gzweb/http/client/assets" >> ~/.bashrc

# run gzserver and gzweb
CMD npm start