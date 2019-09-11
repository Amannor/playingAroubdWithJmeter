# playingAroundWithJmeter
My ham-fisted attempt at concurrent stress test with jmeter

## Background
----------
I needed to do a stress test on a small GCF (the CL) which is just a fancy way to say that I wanted to do a DDos on a http url.

I tried playing around with some tools (e.g. loader.io, curl, postman) and fimnally came to the conculsion that jmeter wil be the best tool (among my considerations were ease of use, ability to perform a distributed "attack", learning curve, etc.)

## Prerequisites
-------------
I used a 64bit Windows (10) laptop.
Make sure you have java installed. I had some trouble with the latest version* (of both jmeter & java) - so after consultation with a colleague that did something similar in the past - I:
	(1) Installed java version 8.0.2210.11
  (2) Downloaded apache-jmeter version 2.13 

\*In case you're interested the bug was https://stackoverflow.com/questions/47180767/apache-jmeter-error-java-version-is-too-low-to-run-jmeter
I tried to run it via the cmd using what they said (java -jar ApacheJMeter.jar) but had trouble to apply that to the parallel running later on.

## Before getting started
----------------------
This guide is far from exhaustive and will not cover the basic how-to of jmeter. There's plenty of online sources, I used the first several lessons of 'JMeter Beginner Tutorial' on YouTube (https://www.youtube.com/watch?v=M-iAXz8vs48&list=PLhW3qG5bs-L-zox1h3eIL7CZh5zJmci4c)

## Setup
-----
Open a cmd window and run the <jmeterHomeDir>\bin\jmeter.bat. This will open a GUI window in which you can config the test. You can use the attached test.jmx file and change it to your liking.

## Running the test
----------------
You can either run it in:
 - GUI mode by clickng the play (load\stress testing should NOT be done in GUI mode)
 - CLI mode. I did it on:
	 - On my laptop like so: 
	 i)  cd into <jmeterHomeDir>\bin
	 ii) Run: jmeter.bat -n -t c:\users\alon_m\Desktop\bugs\35443_CL_loadTest\alon_test.jmx -l c:\users\alon_m\Desktop\bugs\35443_CL_loadTest\results_prod.txt 
	 - Concurrently on 2 machines (I tried to do it virtually but it ended up being easier to have 2 physical laptops). I installed java & downloaded jmeter (like described above), then on each machine I ran
	 <jmeterHomeDir>\bin\jmeter-server.bat (in cmd).
     
     Extract the ip from the output like in the following example:
		If the output: "... Created remote object: UnicastServerRef [liveRef: [endpoint:[10.41.14.6:60423](local),objID:[-6049fc82:16d204dc72c:-7fff, -6886663647144605613]]]  ..."
		Then the ip is: 10.41.14.6
	
  i)   cd into <jmeterHomeDir>\bin
	
  ii)  Change the hosts in the jmeter.properties file to include the ip addresses: 
		 \# Remote Hosts - comma delimited
		 remote_hosts=192.168.2.138,192.168.2.115
	
  iii) Run: jmeter.bat -n -t <full_path_of_jmx_file> -l <full_path_of_file_to_write_the_resultst_to> -r
