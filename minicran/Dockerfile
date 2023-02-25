
FROM r-base:4.2.2 AS cist-ee-minicran

MAINTAINER Sebastien Rey-Coyrehourcq <sebastien.rey-coyrehourcq@univ-rouen.fr>

RUN apt-get update && apt-get install --no-install-recommends -y curl gdebi-core gdal-bin lbzip2 libfftw3-dev libgdal-dev libgeos-dev libgsl0-dev libgl1-mesa-dev libglu1-mesa-dev \
libhdf4-alt-dev libhdf5-dev libjq-dev libpq-dev libproj-dev libprotobuf-dev libnetcdf-dev \
libsqlite3-dev libssl-dev libudunits2-dev netcdf-bin protobuf-compiler sqlite3 tk-dev unixodbc-dev libcurl4-openssl-dev libudunits2-dev git pandoc \
openssh-server apache2 rsync && apt-get autoremove -y && apt-get clean -y

RUN mkdir -p /var/lock/apache2 /var/run/apache2 

COPY ./utils/ ./utils

RUN mkdir -p /var/www/html/MiniCRAN

ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2

RUN Rscript "./utils/minicran.R"

RUN rm /var/www/html/index.html
RUN mv /var/www/html/MiniCRAN/* /var/www/html/

RUN cp ./utils/apache2.conf /etc/apache2/
RUN a2enmod include

RUN echo 'ServerName 127.0.0.1' >> /etc/apache2/apache2.conf

#VOLUME ["/var/www/html"]

EXPOSE 80

CMD ["apache2ctl", "-D", "FOREGROUND"]
