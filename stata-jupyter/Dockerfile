# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
FROM willthelaw/scipy-notebook

LABEL maintainer="Jupyter Project <jupyter@googlegroups.com>"

# Set when building on Travis so that certain long-running build steps can
# be skipped to shorten build time.
ARG TEST_ONLY_BUILD

ENV DEBIAN_FRONTEND noninteractive

USER root

#SRCC add ons
RUN apt-get update && apt-get install -y --no-install-recommends \
    libnss-extrausers

# R pre-requisites
RUN apt-get install -y --no-install-recommends \
    fonts-dejavu \
    tzdata \
    gfortran \
    gcc && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Stata pre-requisites
# get libpng12-0, libxp6, libjpeg62
RUN cd /tmp && \
    wget -q http://security.ubuntu.com/ubuntu/pool/main/libp/libpng/libpng12-0_1.2.54-1ubuntu1.1_amd64.deb && \
    wget -q http://mirrors.kernel.org/ubuntu/pool/main/libx/libxp/libxp6_1.0.2-1ubuntu1_amd64.deb && \
    wget -q http://mirrors.kernel.org/ubuntu/pool/universe/libj/libjpeg6b/libjpeg62_6b2-3_amd64.deb && \
    dpkg -i libpng12-0_1.2.54-1ubuntu1.1_amd64.deb && \
    dpkg -i libxp6_1.0.2-1ubuntu1_amd64.deb && \
    dpkg -i libjpeg62_6b2-3_amd64.deb && \
    rm /tmp/libpng12-0_1.2.54-1ubuntu1.1_amd64.deb && \
    rm /tmp/libxp6_1.0.2-1ubuntu1_amd64.deb && \
    rm /tmp/libjpeg62_6b2-3_amd64.deb && \
    apt-get install -f

# Install saspyfor SAS with custom config
RUN pip install sas_kernel -q
COPY sascfg_personal.py /opt/conda/lib/python3.6/site-packages/saspy/

# SAS pre-requisites
# RUN echo deb http://us.archive.ubuntu.com/ubuntu trusty main universe > /etc/apt/sources.list.d/saslib-source.list
# RUN apt-get update
# RUN apt-get install -y --no-install-recommends libxp6 libjpeg62 libpng12-0

# Install IPYstata
RUN pip install ipystata -q

# configure Stata program path
#RUN python import pandas as pd && \
 #   import ipystata && \
  #  from ipystata.config import config_stata && \
   # config_stata('~/stata/stata-se')

# Julia dependencies
# install Julia packages in /opt/julia instead of $HOME
#ENV JULIA_PKGDIR=/opt/julia
#ENV JULIA_VERSION=0.6.2

#RUN mkdir /opt/julia-${JULIA_VERSION} && \
#    cd /tmp && \
#    wget -q https://julialang-s3.julialang.org/bin/linux/x64/`echo ${JULIA_VERSION} | cut -d. -f 1,2`/julia-${JULIA_VERSION}-linux-x86_64.tar.gz && \
#    echo "dc6ec0b13551ce78083a5849268b20684421d46a7ec46b17ec1fab88a5078580 *julia-${JULIA_VERSION}-linux-x86_64.tar.gz" | sha256sum -c - && \
#    tar xzf julia-${JULIA_VERSION}-linux-x86_64.tar.gz -C /opt/julia-${JULIA_VERSION} --strip-components=1 && \
#    rm /tmp/julia-${JULIA_VERSION}-linux-x86_64.tar.gz
#RUN ln -fs /opt/julia-*/bin/julia /usr/local/bin/julia

# Show Julia where conda libraries are \
#RUN mkdir /etc/julia && \
#    echo "push!(Libdl.DL_LOAD_PATH, \"$CONDA_DIR/lib\")" >> /etc/julia/juliarc.jl && \
#    # Create JULIA_PKGDIR \
#    mkdir $JULIA_PKGDIR && \
#    chown $NB_USER $JULIA_PKGDIR && \
#    fix-permissions $JULIA_PKGDIR

USER $NB_UID

# R packages including IRKernel which gets installed globally.
RUN conda install --quiet --yes \
    'rpy2=2.8*' \
    'r-base=3.4.1' \
    'r-irkernel=0.8*' \
    'r-plyr=1.8*' \
    'r-devtools=1.13*' \
    'r-tidyverse=1.1*' \
    'r-shiny=1.0*' \
    'r-rmarkdown=1.8*' \
    'r-forecast=8.2*' \
    'r-rsqlite=2.0*' \
    'r-reshape2=1.4*' \
    'r-nycflights13=0.2*' \
    'r-caret=6.0*' \
    'r-rcurl=1.95*' \
    'r-crayon=1.3*' \
    'r-randomforest=4.6*' \
    'r-htmltools=0.3*' \
    'r-sparklyr=0.7*' \
    'r-htmlwidgets=1.0*' \
    'r-hexbin=1.27*' && \
    conda clean -tipsy && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

# R packages installation
# RUN Rscript --vanilla -e
#"install.packages(c('acepack','albersusa','BBmisc','bbmle','bdsmatrix','boot','callr','carData','caTools','checkmate','ck37r','classInt','clipr','clubSan','dwich','cluster','cmprsk','condensier','corrplot','covr','cowpl','ot','coxme','cvAUC','data.table','debugme','delayed','DescTools','dlnm','doParallel','e1071','exactRankTests','expm','fastmatch','Formula','frailtypack','future','future.apply','gdata','geepack','ggfortify','ggpubr','ggrepel','ggsci','ggsignif','ggthemes','globals','gnm','gplots','gridExtra','gridisl','gtools','h2o','Hmisc','htmlTable','icd','icd.data','icd9','igraph','ineq','jomo','keras','km.ci','KMsurv','latticeExtra','listenv','lme4','manipulate','maps','maptools','Matching','MatrixModels','maxLik','maxstat','mice','minqa','miscTools','mitml','mlbench','multcomp','multgee','multiwayvcov','mvtnorm','nanotime','NCmisc','nnls','openxlsx','optmatch','origami','pan','pander','pbkrtest','plm','polspline','praise','processx','proftools','pryr','quantreg','qvcalc','RANN','RcppCCTZ','RcppEigen','reader','readstata13','reldist','relimp','reprex','reticulate','rex','rgdal','rgenoud','rgeos','RhpcBLASctl','rio','RItools','RPostgreSQL','sf','sp','spatial','spData','tensorflow','testthat','tfruns','tinytex','translations','udunits2','units','viridis','xts','zeallot','zipcode'), dependencies=TRUE, repos = 'http://cran.us.r-project.org');"

# Add Julia packages. Only add HDF5 if this is not a test-only build since
# it takes roughly half the entire build time of all of the images on Travis
# to add this one package and often causes Travis to timeout.
#
# Install IJulia as jovyan and then move the kernelspec out
# to the system share location. Avoids problems with runtime UID change not
# taking effect properly on the .local folder in the jovyan home dir.
#RUN julia -e 'Pkg.init()' && \
#    julia -e 'Pkg.update()' && \
#    (test $TEST_ONLY_BUILD || julia -e 'Pkg.add("HDF5")') && \
#    julia -e 'Pkg.add("Gadfly")' && \
#    julia -e 'Pkg.add("RDatasets")' && \
#    julia -e 'Pkg.add("IJulia")' && \
#    # Precompile Julia packages \
#    julia -e 'using IJulia' && \
#    # move kernelspec out of home \
#    mv $HOME/.local/share/jupyter/kernels/julia* $CONDA_DIR/share/jupyter/kernels/ && \
#    chmod -R go+rx $CONDA_DIR/share/jupyter && \
#    rm -rf $HOME/.local && \
#    fix-permissions $JULIA_PKGDIR $CONDA_DIR/share/jupyter
