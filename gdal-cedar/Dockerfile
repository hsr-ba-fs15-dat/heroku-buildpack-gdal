FROM mojodna/cedar-stack

ENV DEBIAN_FRONTEND noninteractive

RUN \
  apt-get upgrade -y

ADD ./proj.tar.gz /tmp
ADD ./proj-datumgrid.tar.gz /tmp/proj-4.8.0/nad/
RUN \
  cd /tmp/proj-* && \
  ./configure --without-mutex --prefix=/app/vendor/proj && \
  make install && \
  cd /app/vendor/proj && \
  tar zcf /tmp/proj-cedar.tar.gz .

ADD ./gdal.tar.gz /tmp
RUN \
  sudo apt-get install libxerces-c-dev libxerces-c3.1 -y && \
  cd /tmp/gdal-* && \
  ./configure --prefix=/app/vendor/gdal \
              --with-static-proj4=/app/vendor/proj \
              --with-pcraster=no \
              --with-jasper=no \
              --with-grib=no \
              --with-vfk=no \
              --with-hide-internal-symbols \
              --with-xerces && \
  make install && \
  cd /app/vendor/gdal && \
  cp /usr/lib/libxerces* ./lib && \
  cp /usr/lib/libicu* ./lib && \
  tar zcf /tmp/gdal-cedar.tar.gz .

RUN \
  curl -O http://download.osgeo.org/geos/geos-3.4.2.tar.bz2 && \
  tar -xjvf geos-3.4.2.tar.bz2 && \
  cd geos-3.4.2 && \
  ./configure --prefix=/app/vendor/geos && \
  make && \
  make install && \
  cd /app/vendor/geos && \
  tar zcf /tmp/geos-cedar.tar.gz .
