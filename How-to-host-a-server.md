# Ubuntu/Linux

This page shows how to create environment for running multiple servers.

    Please replace [name] with the name of your server.

## Clone inexor sources

    mkdir -p /srv/inexor/git
    cd /srv/inexor/git
    git clone --recursive https://github.com/inexor-game/code.git inexor-code
    git clone --recursive https://github.com/inexor-game/data.git inexor-data
    git clone --recursive https://github.com/inexor-game/data-additional.git inexor-data-additional

## Build environment and building

    apt-get install git cmake build-essential libsdl2{,-mixer,-image}-dev libgl1-mesa-dev libprotobuf-dev protobuf-compiler libenet-dev libudev-dev libboost-all-dev
    mkdir -p /srv/inexor/build
    cd /srv/inexor/build
    cmake /srv/inexor/git/inexor-code -DBUILD_CLIENT=0 -DBUILD_MASTER=0 -DBUILD_SERVER=1
    cd /srv/inexor/git/inexor-code
    make install

## Create server directory and 

    mkdir -p /srv/inexor/servers
    ln -s /srv/inexor/git/inexor-code /srv/inexor/servers/[name]

    mkdir -p /etc/inexor/[name]/
    cp /srv/inexor/git/inexor-code/server-init.cfg /etc/inexor/[name]/[name].cfg
    nano /etc/inexor/[name]/[name].cfg

    mkdir -p /var/log/inexor/[name]

## Run the server

    cd /srv/inexor/servers/[name]
    bin/linux/x86_64/server -g/var/log/inexor/[name]/[name].log -x/etc/inexor/[name]/[name].cfg >/dev/null &

## Stop the server

    kill `ps ax|grep [name]|grep server|cut -f1 -d" "