FROM ubuntu:xenial


ENV HOME /yup

ENV USER_ID ${USER_ID:-1000}
ENV GROUP_ID ${GROUP_ID:-1000}

RUN groupadd -g ${GROUP_ID} yup \
        && useradd -u ${USER_ID} -g yup -s /bin/bash -m -d /yup yup

RUN echo "deb http://deb.debian.org/debian/ stretch-backports main" >> /etc/apt/sources.list.d/backports.list
RUN echo "deb-src http://deb.debian.org/debian/ stretch-backports main" >> /etc/apt/sources.list.d/backports.list
RUN apt-get update && apt-get install -y --no-install-recommends \
  libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-program-options-dev libboost-test-dev libboost-thread-dev libboost-all-dev unzip libminiupnpc-dev python-virtualenv build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils libdb4.8 libdb4.8++ git net-tools build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils wget
RUN cd /root && git clone https://github.com/notforhire/Yup.git \
  && cd Yup \
  && mycoin_ROOT=$(pwd) \
  && BDB_PREFIX="${mycoin_ROOT}/db4" \
  && mkdir -p $BDB_PREFIX \
  && wget 'http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz' \
  && echo '12edc0df75bf9abd7f82f821795bcee50f42cb2e5f76a6a281b85732798364ef  db-4.8.30.NC.tar.gz' | sha256sum -c \
  && tar -xzvf db-4.8.30.NC.tar.gz \
  &&  cd db-4.8.30.NC/build_unix/ \
  && ../dist/configure --enable-cxx --disable-shared --with-pic --prefix=$BDB_PREFIX \
  && make install \
  && cd $mycoin_ROOT \
#  && chmod +x autogen.sh share/genbuild.sh src/leveldb/bui \
  && ./autogen.sh \
  && ./configure LDFLAGS="-L${BDB_PREFIX}/lib/" CPPFLAGS="-I${BDB_PREFIX}/include/" \
  && make \
  && make install
ENV PATH="/usr/local/bin/:${PATH}"

VOLUME ["/yup"]
EXPOSE 6269 6270

CMD yupd -printtoconsole; sleep infinity
