FROM continuumio/miniconda3
MAINTAINER vsochat@stanford.edu

# docker build -t vanessa/pvacseq .

ENV DEBIAN_FRONTEND noninteractive

# Dependencies for cpanm
RUN apt-get update && apt-get install -y build-essential \
                                         libdbd-mysql-perl \
                                         libmysqlclient-dev \
                                         apt-utils \ 
                                         unzip \
                                         tcsh \
                                         gawk \
                                         vim \
                                         libxi-dev \ 
                                         libxmu-dev \ 
                                         freeglut3-dev \ 
                                         libgsl0-dev \
                                         libnetpbm10-dev \
                                         libplplot-dev \
                                         gfortran

# Updates and Installs
RUN pip install --upgrade pip
RUN pip install scif
ADD pvacseq.scif /pvacseq.scif
RUN /opt/conda/bin/scif install /pvacseq.scif
ENTRYPOINT ["scif"]
