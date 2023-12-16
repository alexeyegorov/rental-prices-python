FROM mcr.microsoft.com/devcontainers/miniconda:0-3

# [Optional] Uncomment to install a different version of Python than the default
# RUN conda install -y python=3.6 \
#     && pip install --no-cache-dir pipx \
#     && pipx reinstall-all

RUN apt-get update \
    && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
        git \
        zsh \
        wget \
        fonts-powerline

ENV USER vscode
ENV HOME /home/${USER}

USER ${USER}

# Copy environment.yml (if found) to a temp location so we update the environment. Also
# copy "noop.txt" so the COPY instruction does not fail if no environment.yml exists.
RUN conda install -n base -c conda-forge mamba
COPY --chown=${USER} environment.yml* .devcontainer/noop.txt ${HOME}/tmp/conda-tmp/
RUN if [ -f "${HOME}/tmp/conda-tmp/environment.yml" ]; then umask 0002 && /opt/conda/bin/mamba env create -f ${HOME}/tmp/conda-tmp/environment.yml; fi \
    && rm -rf ${HOME}/tmp/conda-tmp

RUN \
    # run the installation script  
    wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh || true \
    # install powerlevel10k
    && git clone https://github.com/romkatv/powerlevel10k.git ${HOME}/.oh-my-zsh/custom/themes/powerlevel10k

COPY --chown=${USER} .zshrc .p10k.zsh ${HOME}

# Activate the environment, and make sure it's activated:
RUN conda init zsh && echo "source activate rental-prices" >> ~/.zshrc
