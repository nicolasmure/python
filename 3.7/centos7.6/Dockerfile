FROM centos:7.6.1810

# to make sure that package install will fail when a package doesn't exist
RUN echo "skip_missing_names_on_install=0" | tee -a /etc/yum.conf

ENV PYTHON_VERSION 3.7.4
ENV PYTHON python3.7

# @see https://tecadmin.net/install-python-3-7-on-centos/
# @see https://github.com/docker-library/python/blob/0f3e6b3ee17e0f1aac79cfeef53fbc08a44bec5f/3.7/alpine3.10/Dockerfile
# @see https://stackoverflow.com/a/56088070
RUN set -eux; \
    REMOVABLE_BUILD_DEPS=" \
        bzip2-devel \
        gcc \
        make \
        openssl-devel \
    "; \
    yum install -y \
        ${REMOVABLE_BUILD_DEPS} \
        # the removal of the below libs are resulting in an error :/
        libffi-devel \
    ; \
    cd /tmp; \
    curl -sSLO "https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz"; \
    tar xfz "Python-${PYTHON_VERSION}.tgz"; \
    cd "Python-${PYTHON_VERSION}"; \
    ./configure \
        --build="$(arch)" \
        --enable-loadable-sqlite-extensions \
        --enable-optimizations \
        --enable-shared LDFLAGS="-Wl,-rpath /usr/local/lib" \
        --prefix=/usr/local \
        --without-ensurepip \
    ; \
    make -j "$(nproc)" altinstall; \
    cd /tmp; \
    rm -rf Python-*; \
    "${PYTHON}" -V; \
    yum erase -y ${REMOVABLE_BUILD_DEPS}; \
    yum clean all; \
    rm -rf /var/cache/yum; \
    find /usr/local -depth \
        \( \
            \( -type d -a \( -name test -o -name tests \) \) \
            -o \
            \( -type f -a \( -name '*.pyc' -o -name '*.pyo' \) \) \
        \) -exec rm -rf '{}' + ;

# make some useful symlinks that are expected to exist
RUN cd /usr/local/bin \
    && ln -s "${PYTHON}" python3

# if this is called "PIP_VERSION", pip explodes with "ValueError: invalid truth value '<VERSION>'"
ENV PYTHON_PIP_VERSION 19.2.2

RUN set -eux; \
    cd /tmp; \
    curl -sSLO https://bootstrap.pypa.io/get-pip.py; \
    "${PYTHON}" get-pip.py \
        --disable-pip-version-check \
        --no-cache-dir \
        "pip==${PYTHON_PIP_VERSION}" \
    ; \
    pip -V; \
    find /usr/local -depth \
        \( \
            \( -type d -a \( -name test -o -name tests \) \) \
            -o \
            \( -type f -a \( -name '*.pyc' -o -name '*.pyo' \) \) \
        \) -exec rm -rf '{}' + ; \
    rm -f get-pip.py;
