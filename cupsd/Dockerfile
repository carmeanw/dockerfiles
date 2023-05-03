ARG MAINTAINER
FROM debian:testing-slim
LABEL maintainer=$MAINTAINER

# Install Packages (basic tools, cups, basic drivers, HP drivers)
RUN apt-get update \
&& apt-get install -y \
  sudo \
  whois \
  usbutils \
  cups \
  cups-client \
  cups-bsd \
  cups-filters \
  foomatic-db-compressed-ppds \
  printer-driver-all \
  openprinting-ppds \
  hpijs-ppds \
  hp-ppd \
  hplip \
  smbclient \
  printer-driver-cups-pdf \
&& apt-get clean \
&& rm -rf /var/lib/apt/lists/*

# Copy and run local brother deb files
COPY ./brother-udev-rule-type1-1.0.2-0.all.deb /
RUN dpkg -i /brother-udev-rule-type1-1.0.2-0.all.deb

COPY ./brscan-skey-0.3.1-2.amd64.deb /
RUN dpkg -i /brscan-skey-0.3.1-2.amd64.deb

COPY ./brscan4-0.4.11-1.amd64.deb /
RUN dpkg -i /brscan4-0.4.11-1.amd64.deb

COPY ./hl3180cdwcupswrapper-1.1.4-0.i386.deb /
RUN dpkg -i /hl3180cdwcupswrapper-1.1.4-0.i386.deb

COPY ./hl3180cdwlpr-1.1.3-0.i386.deb /
RUN  dpkg -i /hl3180cdwlpr-1.1.3-0.i386.deb

# Add user and disable sudo password checking
RUN useradd \
  --groups=sudo,lp,lpadmin \
  --create-home \
  --home-dir=/home/print \
  --shell=/bin/bash \
  --password=$(mkpasswd print) \
  print \
&& sed -i '/%sudo[[:space:]]/ s/ALL[[:space:]]*$/NOPASSWD:ALL/' /etc/sudoers

# Copy the default configuration file
COPY --chown=root:lp cupsd.conf /etc/cups/cupsd.conf

# Default shell
CMD ["/usr/sbin/cupsd", "-f"]