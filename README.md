#  Setup for custom AMI on AWS EC2 GPU instance

After starting up a GPU instance (g2.2xlarge / g2.8xlarge) and secure shelling into it:

1. Install Git and pull AWS-setup repository

	```
	sudo apt-get --assume-yes install git
	mkdir AWS_setup
	cd AWS_setup
	git init
	git pull https://github.com/dlacombejr/AWS-setup
	cd ..
	```

2. Install most of the dependencies

	```
	chmod +x AWS_setup/setup.sh
	Yes | AWS_setup/setup.sh
	```

    wait a few seconds for reboot...

3. Install cuDNN 
	    
	1. Download cuDNN after registering as a GPU Computing & Embedded Developer at NIVIDIA
	2. Upload the compressed file from local computer to the instance
		- Convention (using Public DNS):

			```
			scp -i ~/key_location/key.pem ~/cuDNN_file_location/cudnn-7.0-linux-x64-v3.0-prod.tgz ubuntu@ec2-XX-XX-XXX-XX.compute-1.amazonaws.com:~/.
			```

		- Example:

			```
			scp -i dan-key-pair-useast.pem ~/Documents/research/AWS/utilites/cudnn-7.0-linux-x64-v3.0-prod.tgz ubuntu@ec2-52-91-177-35.compute-1.amazonaws.com:~/.
			```

	3. Unpack the library on the instance

		```
		tar -zxf cudnn-7.0-linux-x64-v3.0-prod.tgz
		```

	4. Copy the library files into CUDA's include and lib folders on the instance

		```
		sudo cp cuda/include/cudnn.h /usr/local/cuda-7.5/include
		sudo cp cuda/lib64/libcudnn* /usr/local/cuda-7.5/lib64
		```

<!-- 4. Setup s3fs for Amazon S3 as a local file-system

	1. Get dependencies and s3fs:

		```
		sudo apt-get install automake autotools-dev g++ git libcurl4-gnutls-dev libfuse-dev libssl-dev libxml2-dev make pkg-config
		git clone https://github.com/s3fs-fuse/s3fs-fuse.git
		cd s3fs-fuse
		./autogen.sh
		./configure
		make
		sudo make install
		```

	2. Fuse S3 into remote directory:
		```
		mkdir s3
		echo MYIDENTITY:MYCREDENTIAL > /home/ubuntu/AWS_setup/passwd.txt
		chmod 600 ~/AWS_setup/passwd.txt
		s3fs mybucket /home/ubuntu/s3 -o passwd_file=/home/ubuntu/AWS_setup/passwd.txt -d -d -f -o f2 -o curldbg
		``` -->

4. Setup access credentials for `boto`:

	1. Install `boto`:

		```
		pip install boto
		```

	2. Create site-wide credential settings:

		```
		sudo vi /ect/boto.cfg
		```
		- Paste in the following using the credentials:

		```
		[Credentials]
		aws_access_key_id = <your_access_key_here>
		aws_secret_access_key = <your_secret_key_here>
		```
 
		- Save and exit (escape followed by `:wq`)

5. Save the instance as an image

6. Pull remote repository from Github and watch your code fun faster!

---

Special thanks to the following posts/repositories!
	
[How to install Theano on Amazon EC2 GPU instances for deep learning](http://markus.com/install-theano-on-aws/)

[GPU Powered DeepLearning with NVIDIA DIGITS on EC2](http://www.joyofdata.de/blog/gpu-powered-deeplearning-with-nvidia-digits/)

[Install Caffe on EC2 from scratch (Ubuntu, CUDA 7, cuDNN)](https://github.com/BVLC/caffe/wiki/Install-Caffe-on-EC2-from-scratch-(Ubuntu,-CUDA-7,-cuDNN))

[From instance launch to model accuracy: an AWS/Theano walkthrough](http://blog.eduardovalle.com/2015/08/07/aws-theano-walkthrough/)

<!-- [s3fs-fuse source](https://github.com/s3fs-fuse/s3fs-fuse/wiki/Installation-Notes)

[Howto - Setup s3fs on Ubuntu 11.04 x64](http://www.pophams.com/blog/howto-setups3fsonubuntu1104x64) -->

[boto: A Python interface to Amazon Web Services](http://boto.readthedocs.org/en/latest/)
