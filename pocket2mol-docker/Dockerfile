FROM nvidia/cuda:11.3.1-cudnn8-runtime-ubuntu20.04

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y --no-install-recommends \
    wget git ca-certificates bzip2 util-linux && \
    rm -rf /var/lib/apt/lists/*

# Miniconda 설치 (py38)
ENV CONDA_DIR=/opt/conda
RUN wget -q https://repo.anaconda.com/miniconda/Miniconda3-py38_23.11.0-1-Linux-x86_64.sh -O /tmp/miniconda.sh && \
    bash /tmp/miniconda.sh -b -p $CONDA_DIR && rm /tmp/miniconda.sh
ENV PATH=$CONDA_DIR/bin:$PATH
SHELL ["/bin/bash", "-lc"]

# Pocket2Mol 환경 (README 권장 버전)
RUN conda create -y -n Pocket2Mol python=3.8 && \
    conda run -n Pocket2Mol conda install -y pytorch==1.10.1 cudatoolkit=11.3 -c pytorch -c conda-forge && \
    conda run -n Pocket2Mol conda install -y -c pyg pyg && \
    conda run -n Pocket2Mol conda install -y -c conda-forge rdkit biopython pyyaml easydict python-lmdb tensorboard && \
    conda clean -afy

# 소스 코드를 이미지 안에 클론
WORKDIR /opt
RUN git clone https://github.com/danid77/Pocket2Mol_LNPAI.git Pocket2Mol
WORKDIR /opt/Pocket2Mol
RUN mkdir -p ckpt outputs

# conda 환경을 통해 커맨드를 실행하게 고정 (알파폴드 스타일 실행 지원)
ENTRYPOINT ["conda", "run", "--no-capture-output", "-n", "Pocket2Mol"]
CMD ["bash"]
