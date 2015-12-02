FROM ubuntu:trusty
MAINTAINER Andrew Rothstein andrew.rothstein@gmail.com

RUN apt-get update -y \
 && apt-get upgrade -y \
 && apt-get install -y libreadline-dev libncurses5-dev libpcre3-dev libssl-dev perl make build-essential curl \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV OPENRESTY_BUILDDIR /openresty-build
RUN mkdir -p ${OPENRESTY_BUILDDIR}
WORKDIR ${OPENRESTY_BUILDDIR}

ENV OPENRESTY_VER 1.9.3.2
ENV OPENRESTY_NAME ngx_openresty-${OPENRESTY_VER}
ENV OPENRESTY_TGZ ${OPENRESTY_NAME}.tar.gz
ENV OPENRESTY_MIRROR https://openresty.org/download

RUN curl -sL ${OPENRESTY_MIRROR}/${OPENRESTY_TGZ} | tar -z -x -C $OPENRESTY_BUILDDIR; \
 cd ${OPENRESTY_BUILDDIR}/${OPENRESTY_NAME}; \
 ./configure && make && make install; \
 rm -rf $OPENRESTY_BUILDDIR

ENV OPENRESTY_RUNDIR /openresty
RUN mkdir -p ${OPENRESTY_RUNDIR}
VOLUME ${OPENRESTY_RUNDIR}/conf
VOLUME ${OPENRESTY_RUNDIR}/logs
WORKDIR ${OPENRESTY_RUNDIR}
CMD /usr/local/openresty/nginx/sbin/nginx -p ${OPENRESTY_RUNDIR} -c conf/nginx.conf