
   
FROM harbor.cyverse.org/vice/jupyter/datascience:latest

USER root

# Update the packages
RUN apt-get update \
    && apt-get clean \
    && rm -rf /usr/lib/apt/lists/*

USER jovyan

# Install ipykernel & Biopython
#RUN python3 -m pip install ipykernel \
#    && pip install biopython==1.72

# Install OpenJDK & PySpark
#RUN conda install openjdk=8.0 pyspark=2.4 biopython=1.72 numba=0.49 jupyterlab=3.2 \
#    ipywidgets=7.6 py3Dmol=0.8 py2neo monotonic black -y
#RUN conda install openjdk=8.0 pyspark=2.4 biopython=1.72 py3Dmol=0.8 py2neo monotonic  -y

# Install mmtfPySpark
RUN pip install gql git+https://github.com/sbl-sdsc/mmtf-pyspark.git
WORKDIR /home/jovyan

# Set default environment variables for MMTF Hadoop Sequence files
ARG MMTF_FULL_ENV=/home/jovyan/vice/MMTF_Files/full
ENV MMTF_FULL=$MMTF_FULL_ENV
ARG MMTF_REDUCED_ENV=/home/jovyan/vice/MMTF_Files/reduced
ENV MMTF_REDUCED=$MMTF_REDUCED_ENV

# Set default environment variables for PySpark
ARG MASTER_ENV=local[16]
ENV MASTER=$MASTER_ENV
ARG SPARK_DRIVER_MEMORY_ENV=20G
ENV SPARK_DRIVER_MEMORY=$SPARK_DRIVER_MEMORY_ENV
ARG SPARK_DRIVER_MAXRESULTSIZE_ENV=8G
ENV SPARK_DRIVER_MAXRESULTSIZE=$SPARK_DRIVER_MAXRESULTSIZE_ENV

# Disable multi-threading of Intel MKL and OpenBLAS
ENV OPENBLAS_NUM_THREADS=1
ENV MKL_NUM_THREADS=1
ENV OMP_NUM_THREADS=1

# clone mmtf-pyspark and delete unnecessary files and directories
RUN git clone https://github.com/Brunk-Lab/proteomics-sciomics-workshop.git \
    && rm -rf proteomics-sciomics-workshop/vice

COPY entry.sh /bin
RUN mkdir /home/jovyan/.irods

ENTRYPOINT ["/bin/entry.sh"]