FROM ubuntu:18.04

ENV DEBIAN_FRONTEND noninteractive
ENV EXPOSURE=0
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US.UTF-8
ENV LANG_CODE=eng
ENV LC_ALL=en_US.UTF-8
ENV TESSDATA_PREFIX=/usr/local/share/tessdata
ENV fontname=

WORKDIR /app

COPY requirements.txt ./

RUN apt-get update && \
	apt-get install -y \
		autoconf \
		autoconf-archive \
		automake \
		bc \
		g++ \
		libicu-dev \
		libjpeg8-dev \
		libleptonica-dev \
		libpango1.0-dev \
		libpng-dev \
		libtiff5-dev \
		libtool \
		locales \
		make \
		pkg-config \
		python3-pip \
		software-properties-common \
		unzip \
		vim \
		wget

RUN add-apt-repository ppa:git-core/ppa && \
	apt-get install -y git && \
	git clone --filter=blob:none --sparse https://github.com/tesseract-ocr/langdata_lstm.git && \
	cd langdata_lstm && \
	git sparse-checkout add "eng/"

RUN mkdir model && \
	mkdir src && \
	cd /app/src && \
	wget https://github.com/tesseract-ocr/tesseract/archive/4.1.0.zip && \
	unzip 4.1.0.zip && \
	cd /app/src/tesseract-4.1.0 && \
	./autogen.sh && \
	./configure && \
	make && \
	make install && \
	ldconfig && \
	make training && \
	make training-install && \
	cd /usr/local/share/tessdata && \
	wget https://github.com/tesseract-ocr/tessdata_best/raw/main/eng.traineddata

RUN pip3 install -r requirements.txt && \
	locale-gen en_US.UTF-8

RUN cd src/ && \
	git clone https://github.com/tesseract-ocr/tesstrain.git

CMD tail -f /dev/null
