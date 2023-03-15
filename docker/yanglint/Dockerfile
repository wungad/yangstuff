FROM alpine

RUN \
    apk -U add alpine-sdk cmake pcre2-dev

# fetch
ARG GIT_BRANCH="master"
RUN \
    git clone --depth=1 --branch=$GIT_BRANCH https://github.com/CESNET/libyang.git

# build
ARG CMAKE_BUILD_TYPE="MinSizeRel"
RUN \
    cmake -S libyang -B build \
        -DCMAKE_BUILD_TYPE=$CMAKE_BUILD_TYPE \
        -DENABLE_STATIC=ON && \
    make -C build && strip -s build/yanglint

# take the yanglint binary
FROM scratch
COPY --from=0 /build/yanglint yanglint
