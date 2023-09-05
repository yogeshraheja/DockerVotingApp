# DockerVotingApp

A Python webapp which lets you vote between two options "Thumb up OR Thumb down"
A Redis queue which collects new votes
A .NET worker which consumes votes and stores them in…
A Postgres database backed by a Docker volume
A Node.js webapp which shows the results of the voting in real time

Image Build and Pull:
=====================
docker image build -t vote:v1 .

docker image build -t worker:v1 .

docker image build -t result:v1 .

docker image pull yogeshraheja/redis:v1

docker image pull yogeshraheja/db:v1

So we have our image with us, let us create a user-defined bridge network now

User-defined Bridge Network Creation:
=====================================
docker network create --driver=bridge --subnet=192.168.10.0/24 appnet

So its time to create our container inside this network, we need to make sure to put the name for redis container as redis, worker as worker and database as db as the names are hardcoded in the code itself.

Container Creation:
===================
docker container run -itd --net=appnet --name=vote -p 81:80 vote:v1 

docker container run -itd --net=appnet --name=redis yogeshraheja/redis:v1

docker container run -itd --net=appnet --name=worker worker:v1

docker container run -itd --net=appnet --name=db yogeshraheja/db:v1

docker container run -itd --net=appnet --name=result -p 82:80 result:v1

Now go to browser with your Host IP and access the Vote Front End at port 81 and Result Front End at Port 82.
