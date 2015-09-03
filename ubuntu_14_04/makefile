MAKEFILE=makefile

# The shell variables below will be available for 
# recipe.
export LC_ALL=C
export LANG=C

.PHONY: help bashrc miscellaneous_tools add_pub_key \
	add_repos desktop_and_x2go R_installation R_lib_install \
	Rstudio_install python_2and3_install sickle_install \
	bowtie_install bowtie2_install tophat2_install \
	subread_install fastqc_install deep_tool_install get_all_version

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

EMACS_VER=24
TOPHAT2_VER=2.1.0
RSTUDIO_VER=0.99.473
SICKLE_VER=1.33
BOWTIE1_VER=1.1.2
BOWTIE2_VER=2.2.5
TOPHAT_VER=2.1.0
SUBREAD_VER=1.4.6-p4
FASTQC_VER=v0.11.3
DEEPTOOLS_VER=1.5.11
NUMPY_VER=1.9.2
BXPYTHON_VER=0.7.3
MATPLOTLIB_VER=1.4.3
PYSAM_VER=0.8.3
SCIPY_VER=0.16.0
SAMTOOLS_VER=1.2
BEDTOOLS_VER=2.24.0


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

	@echo "" >> /etc/environment
	@echo "export LC_ALL=C" >> /etc/environment
	@echo "export LANG=C" >> /etc/environment
	@echo 'export PS1="\[\033[01;31m\]\h\[\033[00m\]\[\033[01;32m\] \[\033[01;32m\]\u \[\033[00;33m\]\w\n\[\033[01;30m\e[0m\e[0;30m\]$$ "' >>  /etc/environment
	@echo 'export PATH=/sbin:$PATH' >> /etc/environment
	@echo 'alias ls="ls --color"' >>  /etc/environment
	@echo 'alias rm="rm -i"' >>  /etc/environment

##########################
# Miscellaneous tools
##########################

miscellaneous_tools: 
	apt-get install -y  emacs$(EMACS_VER)-nox gdebi

##########################
# Ubuntu repositories keys
##########################

add_pub_key:
	for i in '$(PUB_KEY)'; do echo "PUB_KEY: $$i"; apt-key adv --recv-keys --keyserver keyserver.ubuntu.com $$i; done

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
	@echo "deb $(CRAN_MIRROR)/bin/linux/ubuntu $(UBUNTU_VER_NAME)/" >> /etc/apt/sources.list
	@apt-get update
	@apt-get -y install r-base r-base-dev libcurl4-openssl-dev libxml2-dev
	@echo "r <- getOption('repos'); r['CRAN'] <- 'http://cran.us.r-project.org'; options(repos = r);" >> ~/.Rprofile


R_lib_install: 
	@Rscript -e "pack.list <- c($(CRAN_PACK_LIST)); \
	pack <- pack.list[!(pack.list %in% installed.packages()[,'Package'])]; \
	if(length(pack)) install.packages(pack); \
	source('http://bioconductor.org/biocLite.R'); \
	pack.list <- c($(BIOC_PACK_LIST)); \
	pack <- pack.list[!(pack.list %in% installed.packages()[,'Package'])]; \
	if(length(pack)) biocLite(pack)"

Rstudio_install: 
	@echo $$PATH
	@wget https://download1.rstudio.org/rstudio-$(RSTUDIO_VER)-amd64.deb
	@yes | PATH=/sbin:$$PATH  gdebi rstudio-$(RSTUDIO_VER)-amd64.deb
	@rm -f rstudio-$(RSTUDIO_VER)-amd64.deb

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
	echo "TOTO" ;\
	mv sickle /usr/local/bin ;\
	cd ..;\
	rm -Rf sickle


bowtie_install: 
	@wget http://downloads.sourceforge.net/project/bowtie-bio/bowtie/$(BOWTIE1_VER)/bowtie-$(BOWTIE1_VER)-linux-x86_64.zip;\
	unzip bowtie-$(BOWTIE1_VER)-linux-x86_64.zip;\
	cd bowtie-$(BOWTIE1_VER);\
	mv bowtie* /usr/local/bin/;\
	cd ..;\
	rm -Rf bowtie-$(BOWTIE1_VER)*

bowtie2_install:
	@wget http://downloads.sourceforge.net/project/bowtie-bio/bowtie2/2.2.5/bowtie2-$(BOWTIE2_VER)-linux-x86_64.zip;\
	unzip bowtie2-$(BOWTIE2_VER)-linux-x86_64.zip;\
	cd bowtie2-$(BOWTIE2_VER);\
	mv bowtie2* /usr/local/bin/;\
	cd ..;\
	rm -Rf bowtie2-$(BOWTIE2_VER)*

tophat2_install:
	@wget https://ccb.jhu.edu/software/tophat/downloads/tophat-$(TOPHAT_VER).Linux_x86_64.tar.gz;\
	tar xvfz tophat-$(TOPHAT_VER).Linux_x86_64.tar.gz;\
	cd tophat-$(TOPHAT_VER).Linux_x86_64; \
	rm -Rf AUTHORS LICENSE README intervaltree/ sortedcontainers/; \
	mv ./* /usr/local/bin;\
	cd ..; rm -Rf tophat-$(TOPHAT_VER).Linux_x86_64*


subread_install:
	@wget http://sourceforge.net/projects/subread/files/subread-$(SUBREAD_VER)/subread-$(SUBREAD_VER)-Linux-x86_64.tar.gz;\
	tar xvfz subread-$(SUBREAD_VER)-Linux-x86_64.tar.gz;\
	cd subread-$(SUBREAD_VER)-Linux-x86_64/bin/;\
	rm -Rf utilities/;\
	mv ./* /usr/local/bin;\
	cd ../..; rm -Rf  subread-$(SUBREAD_VER)-Linux-x86_64*

fastqc_install:
	@wget http://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_$(FASTQC_VER).zip;\
	unzip -o fastqc_$(FASTQC_VER).zip;\
	rm -f fastqc_$(FASTQC_VER).zip;\
	rm -rf /opt/FastQC; mv FastQC /opt;\
	chmod 777 /opt/FastQC/fastqc;\
	echo 'PATH=$$PATH:/opt/FastQC/' >> ~/.bashrc


deep_tool_install: 
	@sudo apt-get install -y python-dev python-pandas
	@apt-get install -y libfreetype6-dev 
	@pip install -I deeptools==$(DEEPTOOLS_VER) \
			numpy==$(NUMPY_VER) bx-python==$(BXPYTHON_VER) \
			matplotlib==$(MATPLOTLIB_VER)  pysam==$(PYSAM_VER)  scipy==$(SCIPY_VER)



samtools_install:
	@wget http://sourceforge.net/projects/samtools/files/samtools/$(SAMTOOLS_VER)/samtools-$(SAMTOOLS_VER).tar.bz2
	@bunzip2 samtools-$(SAMTOOLS_VER).tar.bz2
	@tar xvf samtools-$(SAMTOOLS_VER).tar
	@cd samtools-$(SAMTOOLS_VER); \
		make ;\
		make install
	@rm -rf samtools*

bedtools_install:
	@wget https://github.com/arq5x/bedtools2/releases/download/v$(BEDTOOLS_VER)/bedtools-$(BEDTOOLS_VER).tar.gz
	@tar xvfz bedtools-$(BEDTOOLS_VER).tar.gz
	@cd bedtools2; \
		make ;\
		make install
	@rm -rf bedtools*

snakemake_install:
	@pip3 install snakemake


SICKLE_VER=$(shell sickle --version | grep "sickle version" | sed 's/sickle version //')
PYTHON3_VER=$(shell python3 --version | sed 's/Python //')
PYTHON_VER=$(shell python --version 2>&1 | sed 's/Python //')
EMACS_VER=$(shell emacs --version| head -1 |sed 's/GNU Emacs //')
MATE_DESKTOP=$(shell mate-session  --version| sed 's/mate-session //' )
SNAKEMAKE_VER=$(shell snakemake --version)
get_all_version:
	@echo emacs $(EMACS_VER)
	@echo tophat $(TOPHAT2_VER)
	@echo rstudio $(RSTUDIO_VER)
	@echo sickle $(SICKLE_VER)
	@echo bowtie1 $(BOWTIE1_VER)
	@echo bowtie2 $(BOWTIE2_VER)
	@echo tophat2 $(TOPHAT_VER)
	@echo subread $(SUBREAD_VER)
	@echo python3 $(PYTHON3_VER)
	@echo python $(PYTHON_VER)
	@echo fastqc $(FASTQC_VER)
	@echo deeptools $(DEEPTOOLS_VER)
	@echo numpy $(NUMPY_VER)
	@echo bx-python $(BXPYTHON_VER)
	@echo matplotlib $(MATPLOTLIB_VER)
	@echo pysam $(PYSAM_VER)
	@echo scipy $(SCIPY_VER) 

all: bashrc miscellaneous_tools add_pub_key add_repos \
	desktop_and_x2go R_installation R_lib_install \
	Rstudio_install python_2and3_install sickle_install \
	bowtie_install bowtie2_install tophat2_install \
	subread_install fastqc_install deep_tool_install \
	samtools_install bedtools_install snakemake_install get_all_version
