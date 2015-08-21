MAKEFILE=makefile

export LC_ALL=C # Avalaible for all recipe commands
export LANG=C

.PHONY: fix_local add_pub_key bashrc miscellaneous_tools \
	R_installation R_lib_install python_install

##########################
# help
##########################

help:
	@perl -ne 'if(/^(\w+):/){print "\t",$$1,"\n"}' $(MAKEFILE)

##########################
# Installation variables
##########################

UBUNTU_VER_NAME="trusty"
CRAN_MIRROR="http://cran.univ-lyon1.fr"
CRAN_PACK_LIST='reshape2','plyr', 'ggplot2', 'Rcpp','igraph','knitr','readr','lattice','ggbio','colorspace','data.table','amap','devtools'
BIOC_PACK_LIST='affy', 'biomaRt'
PUB_KEY=51716619E084DAB9 F7B8CEA6056E8E56	

###########################
# Tool versions
###########################

EMACS_VER="24"
TOPHAT2_VER="2.1.0"

##########################
# fix invalid local
##########################

fix_local:
	@echo "" >> /etc/environment
	@echo 'LC_ALL=C' >>  /etc/environment

##########################
# ~/.bashrc config
##########################

bashrc: 
	@echo "" >> ~/.bashrc
	@echo "export LC_ALL=C" >> ~/.bashrc
	@echo "export LANG=C" >> ~/.bashrc
	@echo 'export PS1="\[\033[01;31m\]\h\[\033[00m\]\[\033[01;32m\] \[\033[01;32m\]\u \[\033[00;33m\]\w\n\[\033[01;30m\e[0m\e[0;30m\]$$ "' >>  ~/.bashrc
	@echo 'alias ls="ls --color"' >>  ~/.bashrc
	@echo 'alias rm="rm -i"' >>  ~/.bashrc

##########################
# Miscellaneous tools
##########################

miscellaneous_tools: 
	apt-get install -y  emacs${EMACS_VER}-nox

##########################
# Ubuntu repositories keys
##########################

add_pub_key: 
	for i in '${PUB_KEY}'; do echo "PUB_KEY: $$i"; apt-key adv --recv-keys --keyserver keyserver.ubuntu.com $$i; done

##########################
# Desktop installation
##########################

add_repos:
	@apt-get install --yes python-software-properties #installation de la commande "add-apt-repository"
	@add-apt-repository ppa:x2go/stable --yes;
	@apt-add-repository ppa:ubuntu-mate-dev/ppa --yes;
	@apt-add-repository ppa:ubuntu-mate-dev/trusty-mate --yes;
	@add-apt-repository ppa:ravefinity-project/ppa --yes ;
	@apt-get update --yes

desktop_and_x2go:
	@apt-get install --yes --force-yes x2goserver 
	@apt-get install --yes --no-install-recommends ubuntu-mate-core ubuntu-mate-desktop;
	@apt-get install --yes --force-yes mate-desktop-environment-extra; 
	@apt-get install --yes --force-yes mate-notification-daemon caja-gksu caja-open-terminal;
	@sudo apt-get install --yes ambiance-colors radiance-colors;


##########################
# R installation 
##########################

R_installation:
	@echo "" >> /etc/apt/sources.list
	@echo "deb ${CRAN_MIRROR}/bin/linux/ubuntu ${UBUNTU_VER_NAME}/" >> /etc/apt/sources.list
	@apt-get update
	@apt-get -y install r-base r-base-dev libcurl4-openssl-dev libxml2-dev
	@echo "r <- getOption('repos'); r['CRAN'] <- 'http://cran.us.r-project.org'; options(repos = r);" >> ~/.Rprofile


R_lib_install:
	@Rscript -e "pack.list <- c(${CRAN_PACK_LIST}); \
	pack <- pack.list[!(pack.list %in% installed.packages()[,'Package'])]; \
	if(length(pack)) install.packages(pack); \
	source('http://bioconductor.org/biocLite.R'); \
	pack.list <- c(${BIOC_PACK_LIST}); \
	pack <- pack.list[!(pack.list %in% installed.packages()[,'Package'])]; \
	if(length(pack)) biocLite(pack)"

python_install:
	@#pip install "ipython[notebook]"
	@#apt-get install -qqy python3-setuptools python3-docutils python3-flask python3-
	@#easy_install3 snakemake
