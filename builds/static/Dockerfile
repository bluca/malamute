FROM voidlinux/voidlinux-musl
MAINTAINER Benjamin Henrion <zoobab@gmail.com>

RUN xbps-install -Su
RUN xbps-install -Sy git make gcc gcc-c++ libtool autoconf automake pkg-config

RUN useradd -d /home/zmq -m -s /bin/bash zmq
RUN echo "zmq ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/zmq
RUN chmod 0440 /etc/sudoers.d/zmq

USER zmq

WORKDIR /home/zmq
RUN git clone git://github.com/jedisct1/libsodium.git
WORKDIR /home/zmq/libsodium
RUN ./autogen.sh
RUN ./configure --prefix=/usr
RUN make
RUN sudo make install

WORKDIR /home/zmq
RUN git clone git://github.com/zeromq/libzmq.git
WORKDIR /home/zmq/libzmq
RUN ./autogen.sh
RUN ./configure --prefix=/usr
RUN make
RUN sudo make install

WORKDIR /home/zmq
RUN git clone git://github.com/zeromq/czmq.git
WORKDIR /home/zmq/czmq
RUN ./autogen.sh
RUN ./configure --prefix=/usr
RUN make
RUN sudo make install

WORKDIR /home/zmq
RUN git clone git://github.com/zeromq/malamute.git
WORKDIR /home/zmq/malamute
RUN ./autogen.sh
RUN ./configure --disable-shared --prefix=/usr
# "-s" will strip the symbols and make the binary smaller
RUN make LDFLAGS="-all-static -s"
RUN sudo make install
# ldd returns an exit code of 0 if the binary is dynamic, 1 if it is a static, here the "!" reverts the test to make it successful if it is a static
RUN ! ldd /usr/bin/malamute
