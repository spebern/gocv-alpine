FROM spebern/gocv-alpine:4.6.0-buildstage as build-stage
FROM alpine:3.16

# OpenCV 4.0.1 shared objects from build-stage
COPY --from=build-stage /usr/local/lib /usr/local/lib
COPY --from=build-stage /usr/local/lib/pkgconfig/opencv4.pc /usr/local/lib/pkgconfig/opencv4.pc
COPY --from=build-stage /usr/local/include/opencv4/opencv2 /usr/local/include/opencv4/opencv2

ENV PKG="libstdc++ \
         ca-certificates \
         wget \
         libjpeg-turbo \
         libpng \
         tiff \
         libavc1394 \
         openblas \
         ffmpeg \
         libgphoto2"

RUN apk update && \
    apk upgrade && \
    apk add --no-cache ${PKG} && \
    # The gblic v2.28-r0 doesn't have sgerrand.rsa.pub.
    # Downloading it from previous version.
    wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.27-r0/sgerrand.rsa.pub && \
    wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.28-r0/glibc-2.28-r0.apk && \
    apk add --no-cache glibc-2.28-r0.apk && \
    rm -fr /glibc-2.28-r0.apk && \
    rm -rf /var/cache/apk/* && \
    apk del wget

ENV PKG_CONFIG_PATH /usr/local/lib/pkgconfig
ENV LD_LIBRARY_PATH /usr/local/lib
ENV CGO_CPPFLAGS -I/usr/local/include
ENV CGO_CXXFLAGS "--std=c++1z"
ENV CGO_LDFLAGS "-L/usr/local/lib -lopencv_core -lopencv_face -lopencv_videoio -lopencv_imgproc -lopencv_highgui -lopencv_imgcodecs -lopencv_objdetect -lopencv_features2d -lopencv_video -lopencv_dnn -lopencv_xfeatures2d -lopencv_plot -lopencv_tracking"
