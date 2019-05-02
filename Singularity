Bootstrap: docker
From: ubuntu:latest
%help
Help me. I'm in the container.

%setup


%files


%labels
    Maintainer Shahinur
    Version 1.0

%environment


%post
	apt-get update && apt-get install -y
	apt-get install -y build-essential
	apt-get install -y g++
	apt-get install -y zlib1g-dev
	apt-get install -y git
	apt-get install -y cmake
	apt-get install -y jq
	which cmake
	git config --global url."https://".insteadOf git://
	export SuperBuild_ANTS_USE_GIT_PROTOCOL="OFF"
    mkdir /code 
	cd /code
	git clone https://github.com/stnava/ANTs.git
	mkdir -p /code/bin/ants
	cd /code/bin/ants
	cmake /code/ANTs
	make
	make -j8
	cp /code/ANTs/Scripts/*.* /code/bin/ants/bin/
	export ANTSPATH=/code/bin/ants/bin
	export PATH=${ANTSPATH}:$PATH
	

%runscript
    echo "Arguments received: $*"
	dim=`cat $1 | jq -r '.dim'`
	prefix=`cat $1 | jq -r '.out_file_prefix'`
	fixed_file=`cat $1 | jq -r '.fixed_file'`
	move_file=`cat $1 | jq -r '.move_file'`
	use_hist=`cat $1 | jq -r '.use_hist'`
	trans_type=`cat $1 | jq -r '.trans_type'`
	AP=`cat $1 | jq -r '.antspath'`
	precision=`cat $1 | jq -r '.precision'`
	output_file=`cat $1 | jq -r '.outputfile'`
	ITK_GLOBAL_DEFAULT_NUMBER_OF_THREADS=`cat $1 | jq -r '.num_of_thread'`
	export ITK_GLOBAL_DEFAULT_NUMBER_OF_THREADS
	export ANTSPATH=$AP
	export PATH=${ANTSPATH}:$PATH
	echo "-d $dim -f $fixed_file -m $move_file -j $use_hist -p $precision -t $trans_type -o $output_file${prefix}_reg_by_cbi" 
	antsRegistrationSyN.sh -d $dim -f $fixed_file -m $move_file -j $use_hist -p $precision -t $trans_type -o $output_file${prefix}_reg_by_cbi

