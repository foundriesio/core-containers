FROM alpine:latest
ENV AKTUALIZR_SRCREV 896ca2a38aa497c47599b2e3b16b518ae0e8eccf
WORKDIR /root/

RUN apk add --no-cache cmake git g++ make curl-dev libarchive-dev libsodium-dev dpkg-dev doxygen graphviz sqlite-dev glib-dev autoconf automake libtool python3 \
	&& wget https://sourceforge.net/projects/boost/files/boost/1.57.0/boost_1_57_0.tar.gz \
	&& tar -xzf boost_1_57_0.tar.gz \
	&& cd boost_1_57_0 \
	&& ./bootstrap.sh --with-libraries="log,filesystem,program_options,system" \
	&& mkdir /usr/include/linux \
	&& echo "#define LINUX_VERSION_CODE 266507" >> /usr/include/linux/version.h \
	&& echo "#define KERNEL_VERSION(a,b,c) (((a) << 16) + ((b) << 8) + (c))" >> /usr/include/linux/version.h \
	&& ./b2 -j`getconf _NPROCESSORS_ONLN` install \
	&& cd ../ \
	&& git clone https://github.com/vlm/asn1c \
	&& cd asn1c \
	&& autoreconf -iv \
	&& ./configure \
	&& make -j`getconf _NPROCESSORS_ONLN` install \
	&& cd ../ \
	&& git clone https://github.com/advancedtelematic/aktualizr.git \
	&& cd aktualizr \
	&& git checkout $AKTUALIZR_SRCREV \
	&& git submodule update --init --recursive \
	&& mkdir build-git \
	&& cd build-git \
	&& sed -i '/GLOB_PERIOD/a #define GLOB_TILDE 4096' /usr/include/glob.h \
	&& sed -i '/RSA_R_WRONG_SIGNATURE_LENGTH/a #define RSA_PKCS1_OpenSSL RSA_PKCS1_SSLeay' /usr/include/openssl/rsa.h \
	&& sed -i 's/BIO_new_mem_buf(void/BIO_new_mem_buf(const void/' /usr/include/openssl/bio.h \
	&& cmake -DWARNING_AS_ERROR=OFF -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr -DBUILD_SOTA_TOOLS=ON -DBUILD_SYSTEMD=OFF .. \
	&& make -C src/sota_tools -j`getconf _NPROCESSORS_ONLN`

## Stage 2
FROM alpine:latest
WORKDIR /root/

RUN apk add --no-cache bash glib libarchive libcurl libsodium openjdk8-jre-base ostree
COPY ota-publish.sh /usr/bin/ota-publish
COPY --from=0 /root/aktualizr/build-git/src/sota_tools/garage-check /usr/bin/
COPY --from=0 /root/aktualizr/build-git/src/sota_tools/garage-deploy /usr/bin/
COPY --from=0 /root/aktualizr/build-git/src/sota_tools/garage-push /usr/bin/
COPY --from=0 /root/aktualizr/build-git/src/sota_tools/garage-sign/bin/garage-sign /usr/bin/
COPY --from=0 /root/aktualizr/build-git/src/sota_tools/garage-sign/lib/ /usr/lib/
CMD bash
