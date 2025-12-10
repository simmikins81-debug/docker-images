FROM debian:12.11-slim@sha256:6ac2c08566499cc2415926653cf2ed7c3aedac445675a013cc09469c9e118fdd

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV PATH=/opt/conda/bin:$PATH

# renovate: datasource=custom.anaconda_installer
ARG INSTALLER_URL_LINUX64="https://repo.anaconda.com/archive/Anaconda3-2025.12-1-Linux-x86_64.sh"
ARG SHA256SUM_LINUX64="132f1f312d05e391906b959ad83aa411b97ece55966bb34df011ef41ba60a35c"
# renovate: datasource=custom.anaconda_installer
ARG INSTALLER_URL_S390X="https://repo.anaconda.com/archive/Anaconda3-2024.10-1-Linux-s390x.sh"
ARG SHA256SUM_S390X="e00bd5e6c275695e8050a45aa85790315f504c95243dfe3632f505284310f3c4"
# renovate: datasource=custom.anaconda_installer
ARG INSTALLER_URL_AARCH64="https://repo.anaconda.com/archive/Anaconda3-2025.12-1-Linux-aarch64.sh"
ARG SHA256SUM_AARCH64="9ba6b32919dfd1964377a4ac5c5e4de645b6bd09efab4a25aaf6e121a60e525c"

# hadolint ignore=DL3008
RUN set -x && \
    apt-get update --fix-missing && \
    apt-get install -y --no-install-recommends \
        bzip2 \
        ca-certificates \
        git \
        libglib2.0-0 \
        libsm6 \
        libxcomposite1 \
        libxcursor1 \
        libxdamage1 \
        libxext6 \
        libxfixes3 \
        libxi6 \
        libxinerama1 \
        libxrandr2 \
        libxrender1 \
        mercurial \
        openssh-client \
        procps \
        subversion \
        wget \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* && \
    UNAME_M="$(uname -m)" && \
    if [ "${UNAME_M}" = "x86_64" ]; then \
        INSTALLER_URL=${INSTALLER_URL_LINUX64}; \
        SHA256SUM=${SHA256SUM_LINUX64}; \
    elif [ "${UNAME_M}" = "s390x" ]; then \
        INSTALLER_URL=${INSTALLER_URL_S390X}; \
        SHA256SUM=${SHA256SUM_S390X}; \
    elif [ "${UNAME_M}" = "aarch64" ]; then \
        INSTALLER_URL=${INSTALLER_URL_AARCH64}; \
        SHA256SUM=${SHA256SUM_AARCH64}; \
    fi && \
    wget "${INSTALLER_URL}" -O anaconda.sh -q && \
    echo "${SHA256SUM} anaconda.sh" > shasum && \
    sha256sum --check --status shasum && \
    /bin/bash anaconda.sh -b -p /opt/conda && \
    rm anaconda.sh shasum && \
    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate" >> ~/.bashrc && \
    find /opt/conda/ -follow -type f -name '*.a' -delete && \
    find /opt/conda/ -follow -type f -name '*.js.map' -delete && \
    /opt/conda/bin/conda clean -afy

CMD [ "/bin/bash" ]
