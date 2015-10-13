#  AMI-EC2  SETUP 

1. Install Git and pull AWS-setup repository

	    sudo apt-get --assume-yes install git
	    mkdir AWS_setup
	    cd AWS_setup
	    git init
	    git pull https://github.com/dlacombejr/AWS-setup
	    cd ..

2. Install first set of dependencies

	    chmod +x setup1.sh
	    Yes | ./setup1.sh

    wait a few seconds for reboot...

3. Install second set of dependencies

	    chmod +x setup2.sh
	    ./setup2.sh

