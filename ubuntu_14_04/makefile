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
RSTUDIO_VER="0.99.473"
SICKLE_VER="1.33"
BOWTIE1_VER="1.1.2"
BOWTIE2_VER="2.2.5"
TOPHAT_VER="2.1.0"
SUBREAD_VER="1.4.6-p4"
PYTHON3="3.4.0"
PYTHON="2.7.6"


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
	@echo 'export PATH=/sbin:$PATH' >> ~/.bashrc
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

Rstudio_install:
	@wget https://download1.rstudio.org/rstudio-${RSTUDIO_VER}-amd64.deb
	@yes | PATH=/sbin:$PATH  gdebi rstudio-${RSTUDIO_VER}-amd64.deb
	@rm -f rstudio-${RSTUDIO_VER}-amd64.deb

python_2and3_install:
	@apt-get install ipython-notebook python-matplotlib python-pip
	@apt-get install ipython3-notebook python3-matplotlib python3-pip
	@pip3 install snakemake docutils

###########
# NGS
###########

sickle_install:
	@git clone https://github.com/najoshi/sickle.git; \
		cd sickle;\
		make; \ 
		mv sickle /usr/local/bin ;\
		cd ..;\
		rm -Rf sickle; \

bowtie_install:
	@wget http://downloads.sourceforge.net/project/bowtie-bio/bowtie/${BOWTIE1_VER}/bowtie-${BOWTIE1_VER}-linux-x86_64.zip
	@unzip bowtie-${BOWTIE1_VER}-linux-x86_64.zip
	@cd bowtie-${BOWTIE1_VER}
	@mv bowtie* /usr/local/bin/
	@cd ..: rm -Rf bowtie-${BOWTIE1_VER}

bowtie2_install:
	@wget http://downloads.sourceforge.net/project/bowtie-bio/bowtie2/2.2.5/bowtie2-${BOWTIE2_VER}-linux-x86_64.zip
	@unzip bowtie2-${BOWTIE2_VER}-linux-x86_64.zip
	@cd bowtie2-${BOWTIE2_VER}
	@mv bowtie2* /usr/local/bin/
	@cd ..: rm -Rf bowtie2-${BOWTIE2_VER}

tophat2_install:
	@wget https://ccb.jhu.edu/software/tophat/downloads/tophat-${TOPHAT_VER}.Linux_x86_64.tar.gz
	@tar xvfz tophat-${TOPHAT_VER}.Linux_x86_64.tar.gz
	@rm -Rf AUTHORS LICENSE README intervaltree/ sortedcontainers/
	@mv ./* /usr/local/bin

subread_install:
	@wget http://sourceforge.net/projects/subread/files/subread-${SUBREAD_VER}/subread-${SUBREAD_VER}-Linux-x86_64.tar.gz
	@tar xvfz subread-${SUBREAD_VER}-Linux-x86_64.tar.gz
	@cd subread-${SUBREAD_VER}-Linux-x86_64/bin/
	@rm -Rf utilities/
	@mv ./* /usr/local/bin
	
fastqc_install:
	@echo "todo"
	
