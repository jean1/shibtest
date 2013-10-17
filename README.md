Shibtest is a simple script to test if a Shibboleth IDP is working.

It was developped to automatically ensure that the IDP was still running
event after a configuration modification.

You need to create a test account in your directory and set
a password.

There are 2 building blocks running on 2 different hosts :
the service to test (running on a web server), and the test process itself
(could be installed on a network management host).

1. Test service

It is a simple web application that uses the  Shibboleth SP (service
provider) and output the arguments
The SP metadatas must be known by the IDP, wether
integrated in a federation or directly into the IDP configuration.
You should configure the application to output a few attributes.
The test result consist in getting those attributes displayed
or not.

2. Test process 

You need to do other things on the host that will run the test:

- install dependencies
	sudo apt-get install libcurl4-openssl-dev

- download webisoget (software by Jim Fox from the University of Washington)
	wget http://staff.washington.edu/fox/webisoget/webisoget-2.4.tar.gz

- compile and install it
	tar xf webisoget-2.4.tar.gz
	cd webisoget-2.4
	./configure
	make
	make install

- create webisoget form (see example directory)
	mkdir -p /local/etc/webisoget
	vi /local/etc/webisoget/test.login

	domain=cas.example.com; username=shibusertest; password=XXXXXXXXXXXXXX;
	domain=idp.example.com; "submit_value=Continue;"

- configure url
	vi /local/etc/webisoget/test.url

	https://shibsup.example.com/secure/

- run the test 
	./bin/shibtest
