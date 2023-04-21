FROM rocker/rstudio:4.2.0

LABEL org.opencontainers.image.authors="AIITO <!ITSupport@alleninstitute.org>"
LABEL description="Run RStudio in a container"

RUN apt-get clean all && \
    apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y \
        libhdf5-dev \
        libpq-dev \
        libcurl4-gnutls-dev \
        libssl-dev \
        libxml2-dev \
        libpng-dev \
        libxt-dev \
        zlib1g-dev \
        libbz2-dev \
        liblzma-dev \
        libglpk40 \
        libgit2-28 \
    && apt-get clean all \
    && apt-get purge \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN Rscript -e "install.packages(c('rmarkdown', 'tidyverse', 'workflowr', 'BiocManager'), repo='https://ftp.osuosl.org/pub/cran', Ncpus=4);"
RUN Rscript -e "BiocManager::install()"
RUN Rscript -e 'BiocManager::install("rhdf5", Ncpus=24)'
RUN Rscript -e 'BiocManager::install("biostrings", Ncpus=24)'
