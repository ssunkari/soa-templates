# soa-templates
1. Nodejs/Express Template
   To build docker image locally for simple express app run below command from current working dir
   sudo docker build -t "tag" .
   To run the express app the docker file exposes port 3000 hence run below command
   sudo docker run -p 3000:3000 "tag"
   
