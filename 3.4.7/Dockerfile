FROM frolvlad/alpine-java
 


LABEL maintainer="Alexey Shcherbyna <alexshcherb84@gmail.com>"

ENV LANG C.UTF-8

ENV OPENCV_VERSION 3.4.7
ENV OPENCV_SRC_URL "https://github.com/opencv/opencv/archive/$OPENCV_VERSION.tar.gz"
ENV OPENCV_INSTALL_PREFIX "/usr/local/opencv"
ENV OPENCV_JAVA_PATH "$OPENCV_INSTALL_PREFIX/share/OpenCV/java"

RUN mkdir /opencv_contrib
RUN wget https://github.com/opencv/opencv_contrib/archive/3.4.7.zip -P /opencv_contrib

RUN mkdir /opencv_contrib/3.4.7
RUN unzip /opencv_contrib/3.4.7.zip -d /opencv_contrib/3.4.7

RUN set -x \
  && apk add --no-cache libstdc++

RUN set -x \
    && apk add --no-cache --virtual .build-dependencies \
        cmake \
        tar \
        ca-certificates \
        make \
        g++ \
        linux-headers \
        python \
    && mkdir -p /tmp/ant && cd /tmp/ant \
    && wget -O apache-ant.tar.gz "http://archive.apache.org/dist/ant/binaries/apache-ant-1.10.1-bin.tar.gz" \
    && tar -xzf apache-ant.tar.gz --strip-components=1 \
    && export ANT_HOME=/tmp/ant \
    && export PATH=$PATH:$ANT_HOME/bin \
    && mkdir -p /tmp/opencv && cd /tmp/opencv \
    && wget -O opencv-src.tar.gz "$OPENCV_SRC_URL" \
    && tar -xzf opencv-src.tar.gz --strip-components=1 \
    && mkdir build && cd build \
    && cmake \
        -D CMAKE_BUILD_TYPE=Release \
        -D CMAKE_INSTALL_PREFIX="$OPENCV_INSTALL_PREFIX" \
        -D BUILD_SHARED_LIBS=OFF \
        -D BUILD_EXAMPLES=OFF \
        -D BUILD_TESTS=OFF \
        -D BUILD_PERF_TESTS=OFF \
        -D BUILD_JAVA=ON \
        -D OPENCV_EXTRA_MODULES_PATH="/opencv_contrib/3.4.7/opencv_contrib-3.4.7/modules" \
        -D BUILD_opencv_dnn=ON \
        .. \
    && echo $JAVA_HOME \
    && echo $PATH \
    && make -j$(nproc) \
    && make install \
    && cd / \
    && apk del .build-dependencies \
    && rm -r /tmp/*

CMD /bin/sh

#Disable CUDA explicitly
#-D WITH_CUDA=OFF 
#-D WITH_CUDNN=OFF
#-D WITH_CUBLAS=OFF 
#-D OPENCV_DNN_CUDA=OFF
