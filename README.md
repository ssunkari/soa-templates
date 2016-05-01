# soa-templates
1. Nodejs/Express Template
   To build docker image locally for simple express app run below command from current working dir
   sudo docker build -t "tag" .
   To run the express app the docker file exposes port 3000 hence run below command
   sudo docker run -p 3000:3000 "tag"

   docker-compose scaling web service demo
   A short demo on how to use docker-compose to create a Web Service connected to a load balancer and a Redis Database. Be sure to check out my blog post on the full overview - brianchristner.io
   
   Install
   The instructions assume that you have already installed Docker and Docker Compose.
   
   In order to get started be sure to clone this project onto your Docker Host. Create a directory on your host. Also, when you scale your services it will then tack on a number to the end of the service you scale.
   
   How to get up and running
   Once you've cloned the project to your host we can now start our demo project. Easy! Navigate to the directory in which you cloned the project. Run the following commands from this directory
   
   docker-compose up -d
   The docker-compose command will pull the images from Docker Hub and then link them together based on the information inside the docker-compose.yml file. This will create ports, links between containers, and configure applications as requrired. After the command completes we can now view the status of our stack
   
   docker-compose ps
   
   Scaling
   Now comes the fun part of compose which is scaling. Let's scale our web service from 1 instance to 5 instances. This will now scale our web service container. We now should run an update on our stack so the Loadbalancer is informed about the new web service containers.
   
   docker-compose scale web=5
   Now run our curl command again on our web services and we will now see the number of times increase and the hostname change. To get a deeper understanding tail the logs of the stack to watch what happens each time you access your web services.
   
   docker-compose logs
   Here's the output from my docker-compose logs after I curled my application 5 times so it is clear that the round-robin is sent to all 5 web service containers.
   
   web_5   | 172.17.1.140 - - [04/Sep/2015 14:11:34] "GET / HTTP/1.1" 200 -
   web_1   | 172.17.1.140 - - [04/Sep/2015 14:11:43] "GET / HTTP/1.1" 200 -
   web_2   | 172.17.1.140 - - [04/Sep/2015 14:11:46] "GET / HTTP/1.1" 200 -
   web_3   | 172.17.1.140 - - [04/Sep/2015 14:11:48] "GET / HTTP/1.1" 200 -
   web_4   | 172.17.1.140 - - [04/Sep/2015 14:14:19] "GET / HTTP/1.1" 200 -
   
   Version 2 Compose File
   Version 2 docker-compose file is now available. In order to use the 'docker-compose-v2.yml' file the command changes slightly. Run the below command to launch a version 2 compose project. The v2 is now running the HAproxy from dockercloud.
   
   A benefit of running this load balancer is it automatically detects the coming and going of containers and doesn't require any changes.
   
   Run the below command to launch a version 2 compose project in the foreground.
   
   docker-compose -f docker-compose-v2.yml up
   Open another terminal window to scale and run:
   
   docker-compose -f docker-compose-v2.yml scale web=5
