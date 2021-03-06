MAKEFILE=makefile

# The shell variables below will be available for 
# recipe.
export LC_ALL=C
export LANG=C
export PATH=/ifb/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games

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
# Install genomes of interest
##########################

#SPECIES=hg38
#SPECIES=hg19
SPECIES=sacCer3

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
MACS_VER=1.4.2
JAVA_VER=oracle-java9
IGV_VER=2.3.59
IGVTOOLS_VER=2.3.57
RSEQC_VER=2.6.2

##########################
# ~/.bashrc config
##########################

bashrc:
	@echo "" >> /etc/bash.bashrc
	@echo 'alias ls="ls --color"' >> /etc/bash.bashrc
	@echo 'alias rm="rm -i"' >> /etc/bash.bashrc
	@echo "" >> /etc/bash.bashrc
	@echo "export LC_ALL=C" >> /etc/bash.bashrc
	@echo "export LANG=C" >> /etc/bash.bashrc
	@echo 'export force_color_prompt=yes' >> /etc/bash.bashrc
	@echo 'export LC_COLLATE=C' >> /etc/bash.bashrc
	@echo "" >> /etc/profile
	@echo 'source /etc/bash.bashrc' >> /etc/profile


##########################
# Ubuntu repositories keys
##########################

add_pub_key:
	for i in '$(PUB_KEY)'; do sudo apt-key adv --keyserver  keyserver.ubuntu.com   --recv-keys $$i; done

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
# Miscellaneous tools
##########################

miscellaneous_tools: 
	apt-get install -y  emacs24-nox gdebi


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
	@apt-get install -y libjpeg62
	@wget --no-clobber https://download1.rstudio.org/rstudio-$(RSTUDIO_VER)-amd64.deb
	@yes | PATH=/sbin:$$PATH  gdebi rstudio-$(RSTUDIO_VER)-amd64.deb
	@rm -f rstudio-$(RSTUDIO_VER)-amd64.deb

python_2and3_install: 
	@apt-get -y  install ipython-notebook python-matplotlib python-pip
	@apt-get -y install ipython3-notebook python3-matplotlib python3-pip
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
	@wget --no-clobber http://downloads.sourceforge.net/project/bowtie-bio/bowtie/$(BOWTIE1_VER)/bowtie-$(BOWTIE1_VER)-linux-x86_64.zip;\
	unzip bowtie-$(BOWTIE1_VER)-linux-x86_64.zip;\
	cd bowtie-$(BOWTIE1_VER);\
	mv bowtie* /usr/local/bin/;\
	cd ..;\
	rm -Rf bowtie-$(BOWTIE1_VER)*

bowtie2_install:
	@wget --no-clobber http://downloads.sourceforge.net/project/bowtie-bio/bowtie2/2.2.5/bowtie2-$(BOWTIE2_VER)-linux-x86_64.zip;\
	unzip bowtie2-$(BOWTIE2_VER)-linux-x86_64.zip;\
	cd bowtie2-$(BOWTIE2_VER);\
	mv bowtie2* /usr/local/bin/;\
	cd ..;\
	rm -Rf bowtie2-$(BOWTIE2_VER)*

tophat2_install:
	@wget --no-clobber https://ccb.jhu.edu/software/tophat/downloads/tophat-$(TOPHAT_VER).Linux_x86_64.tar.gz;\
	tar xvfz tophat-$(TOPHAT_VER).Linux_x86_64.tar.gz;\
	cd tophat-$(TOPHAT_VER).Linux_x86_64; \
	rm -Rf AUTHORS LICENSE README intervaltree/ sortedcontainers/; \
	mv ./* /usr/local/bin;\
	cd ..; rm -Rf tophat-$(TOPHAT_VER).Linux_x86_64*


subread_install:
	@wget --no-clobber http://sourceforge.net/projects/subread/files/subread-$(SUBREAD_VER)/subread-$(SUBREAD_VER)-Linux-x86_64.tar.gz;\
	tar xvfz subread-$(SUBREAD_VER)-Linux-x86_64.tar.gz;\
	cd subread-$(SUBREAD_VER)-Linux-x86_64/bin/;\
	rm -Rf utilities/;\
	mv ./* /usr/local/bin;\
	cd ../..; rm -Rf  subread-$(SUBREAD_VER)-Linux-x86_64*

fastqc_install:
	@wget --no-clobber http://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_$(FASTQC_VER).zip;\
	unzip -o fastqc_$(FASTQC_VER).zip;\
	rm -f fastqc_$(FASTQC_VER).zip;\
	rm -rf /opt/FastQC; mv FastQC /opt;\
	chmod 777 /opt/FastQC/fastqc;\
	echo 'PATH=$$PATH:/opt/FastQC/' >> ~/.bashrc

kent_tools_install:
	@mkdir -p kent; cd kent; \
	wget ftp://hgdownload.soe.ucsc.edu/admin/exe/linux.x86_64/bed* ; \
	wget ftp://hgdownload.soe.ucsc.edu/admin/exe/linux.x86_64/big* ; \
	wget ftp://hgdownload.soe.ucsc.edu/admin/exe/linux.x86_64/gff* ; \
	wget ftp://hgdownload.soe.ucsc.edu/admin/exe/linux.x86_64/gff* ; \
	wget ftp://hgdownload.soe.ucsc.edu/admin/exe/linux.x86_64/maf* ; \
	wget ftp://hgdownload.soe.ucsc.edu/admin/exe/linux.x86_64/wig* ; \
	chmod 777 ./* ;\
	mv ./* /usr/local/bin; \
	cd ..; 

deep_tool_install: 
	@sudo apt-get install -y python-dev python-pandas
	@apt-get install -y libfreetype6-dev 
	@pip install -I numpy==$(NUMPY_VER) bx-python==$(BXPYTHON_VER) \
			matplotlib==$(MATPLOTLIB_VER)  pysam==$(PYSAM_VER)  scipy==$(SCIPY_VER)
	@git clone https://github.com/fidelram/deepTools.git; \
	cd deepTools; python setup.py install



samtools_install:
	@wget --no-clobber http://sourceforge.net/projects/samtools/files/samtools/$(SAMTOOLS_VER)/samtools-$(SAMTOOLS_VER).tar.bz2
	@bunzip2 -f samtools-$(SAMTOOLS_VER).tar.bz2
	@tar xvf samtools-$(SAMTOOLS_VER).tar
	@cd samtools-$(SAMTOOLS_VER); \
		make ;\
		make install
	@rm -rf samtools*

bedtools_install:
	@wget --no-clobber https://github.com/arq5x/bedtools2/releases/download/v$(BEDTOOLS_VER)/bedtools-$(BEDTOOLS_VER).tar.gz
	@tar xvfz bedtools-$(BEDTOOLS_VER).tar.gz
	@cd bedtools2; \
		make ;\
		make install
	@rm -rf bedtools*

snakemake_install:
	@pip3 install snakemake

macs_install:
	@wget --no-clobber https://github.com/downloads/taoliu/MACS/MACS-$(MACS_VER)-1.tar.gz
	@tar xvfz MACS-$(MACS_VER)-1.tar.gz 
	@cd MACS-$(MACS_VER); python setup.py install
	@rm -Rf MACS-$(MACS_VER)*

java9_install:
	@echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections; \
	echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections;\
	add-apt-repository -y ppa:webupd8team/java; \
	apt-get update ;\
	apt-get -y install oracle-java9-installer ;\
	apt-get install oracle-java9-set-default

igv_install:
	@wget --no-clobber http://data.broadinstitute.org/igv/projects/downloads/IGV_$(IGV_VER).zip; \
	mv IGV_$(IGV_VER).zip /opt;\
	cd /opt; unzip  IGV_$(IGV_VER).zip;\
	cd IGV_$(IGV_VER);\
	echo "" >> /etc/bash.bashrc;\
	echo "alias igv='cd /opt/IGV_$(IGV_VER); ./igv.sh'" >> /etc/bash.bashrc

igv_tools_install:
	@wget --no-clobber http://data.broadinstitute.org/igv/projects/downloads/igvtools_$(IGVTOOLS_VER).zip ;\
	mv igvtools_$(IGVTOOLS_VER).zip /opt;\
	cd /opt; unzip igvtools_$(IGVTOOLS_VER).zip;\
	echo "" >> /etc/bash.bashrc;\
	echo "alias igvtools='cd /opt/IGVTools; ./igvtools'" >> /etc/bash.bashr

rseqc_install:
	@wget --no-clobber http://sourceforge.net/projects/rseqc/files/RSeQC-$(RSEQC_VER).tar.gz;\
	tar xvfz RSeQC-$(RSEQC_VER).tar.gz;
	cd  RSeQC-$(RSEQC_VER); python setup.py install ; cd ..; rm -Rf RSeQC-$(RSEQC_VER)*

deeptools_install:
	@git clone https://github.com/fidelram/deepTools ;\
	cd deepTools ;\
	python setup.py install; cd ..; \
	rm -Rf deeptools

get_all_version:
	SICKLE_VER=$(shell sickle --version | grep "sickle version" | sed 's/sickle version //')
	PYTHON3_VER=$(shell python3 --version | sed 's/Python //')
	PYTHON_VER=$(shell python --version 2>&1 | sed 's/Python //')
	EMACS_VER=$(shell emacs --version| head -1 |sed 's/GNU Emacs //')
	MATE_DESKTOP=$(shell mate-session  --version| sed 's/mate-session //' )
	SNAKEMAKE_VER=$(shell snakemake --version)
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
	@echo RSeQC $(RSEQC_VER)
	@echo igv $(IGV_VER)
	@echo igv $(IGVTOOLS_VER)


################################################################
## Intall genePredToGtf tool from Jim Kent
################################################################

install_genepred:
	@echo "Installing genPredToGtf from the Jim Kent's legendary toolkit" ; \
	wget --no-clobber http://hgdownload.soe.ucsc.edu/admin/exe/linux.x86_64/genePredToGtf; \
	chmod 755 genePredToGtf ; \
	mv -f genePredToGtf /usr/local/bin

config_db_ucsc:
	@echo "Generating config file for UCSC database queries" ; \
	echo "db.host=genome-mysql.cse.ucsc.edu" > ~/.hg.conf ; \
	echo "db.user=genomep" >> ~/.hg.conf ; \
	echo "db.password=password" >> ~/.hg.conf ; \
	chmod 600 ~/.hg.conf; \
	echo "	~/.hg.conf"



all_install: bashrc  add_pub_key add_repos \
	desktop_and_x2go miscellaneous_tools R_installation R_lib_install \
	Rstudio_install python_2and3_install sickle_install \
	bowtie_install bowtie2_install tophat2_install \
	subread_install fastqc_install kent_tools_install deep_tool_install \
	samtools_install bedtools_install snakemake_install macs_install \
	java9_install igv_install igv_tools_install rseqc_install  get_all_version install_genepred \
	config_db_ucsc	

################################
## Install one genome from UCSC
################################

GOLDEN_PATH_URL=http://hgdownload.cse.ucsc.edu/goldenPath
UCSC_BASE_URL=$(GOLDEN_PATH_URL)/$(SPECIES)/bigZips/
#UCSC_FASTA=$(UCSC_BASE_URL)/$(SPECIES).fa.gz
FASTA_ARCHIVE=chromFa.tar.gz
UCSC_FASTA=$(UCSC_BASE_URL)/$(FASTA_ARCHIVE)
SPECIES_DIR=/root/mydisk/genomes/$(SPECIES)
GENOME_SEQ=$(SPECIES)_genome
GENOME_FASTA=$(GENOME_SEQ).fasta
CHROM_INFO=chromInfo.txt
CHROM_INFO_ARCHIVE=$(CHROM_INFO).gz
BOF=http://hgdownload.soe.ucsc.edu/goldenPath
CHROM_INFO_URL=$(GOLDEN_PATH_URL)/$(SPECIES)/database/$(CHROM_INFO)
REFGENE=refGene.txt
REFGENE_ARCHIVE=$(REFGENE).gz
REFGENE_URL=http://hgdownload.soe.ucsc.edu/goldenPath/$(SPECIES)/database/$(REFGENE_ARCHIVE)
GTF_FILE=$(SPECIES)_refGene.gtf

one_species_from_ucsc:
	@echo "Installing species from UCSC	$(SPECIES)"
	@echo "	SPECIES_DIR	$(SPECIES_DIR)"
	@mkdir -p $(SPECIES_DIR) ; \
	cd $(SPECIES_DIR) ; \
	echo "	Downloading genome sequence	$(UCSC_FASTA)" ; \
	wget --no-clobber $(UCSC_FASTA) ; \
	echo "	Uncompressing archive $(FASTA_ARCHIVE)" ; \
	tar xvfz $(FASTA_ARCHIVE) &> tar_log.txt; \
	cat *.fa > $(GENOME_FASTA); \
	rm -f *.fa ; \
	echo "Building index for bowtie2" ; \
	bowtie2-build -f $(GENOME_FASTA) $(GENOME_SEQ) &> bowtie2_log.txt ; \
	echo "Building index for bowtie" ; \
	bowtie-build -f $(GENOME_FASTA) $(GENOME_SEQ) &> bowtie_log.txt ; \
	echo "	SPECIES_DIR	$(SPECIES_DIR)"
	echo "	Indexing fasta with samtools" ; \
	samtools faidx $(GENOME_FASTA) ; \
	echo "	FASTA index	$(SPECIES_DIR)/$(GENOME_FASTA).fai" ; \
	echo "	Getting chromosome info	$(CHROM_INFO_URL)" ; \
	wget --no-clobber $(CHROM_INFO_URL) ; \
	gunzip -f $(CHROM_INFO_ARCHIVE) ; \
	echo "	CHROM_INFO	$(SPECIES_DIR)/$(CHROM_INFO)" ; \
	echo "	Downloding  GTF file" ; \
	genePredToGtf  $(SPECIES) refGene $(GTF_FILE) ; \
	echo "	 GTF_FILE	$(SPECIES_DIR)/$(GTF_FILE)" ; \
	echo "	Downloading genome annotations	$(REFGENE_URL)" ; \
	wget --no-clobber $(REFGENE_URL) ; \
	echo "	Uncompressing archive $(REFGENE_ARCHIVE)" ; \
	gunzip -f $(REFGENE_ARCHIVE) &> refgene_tar_log.txt; \
	echo "	REFGENE	$(SPECIES_DIR)/$(REFGENE)" ; \
	echo "JOB DONE" ; \
	date

it: 
	@mkdir -p $(SPECIES_DIR) ; \
	cd $(SPECIES_DIR) ; \
