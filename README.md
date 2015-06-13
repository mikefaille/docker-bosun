
# docker-bosun

This docker image contains all the dependencies needed for a bosun server. It is designed for test use and is not suitable for production use since data is not persisted. OpenTSDB and HBase can take 15 or more seconds to initialize after the image is running.  

## Usage  

```
$ docker run -d -p 4242:4242 -p 8070:8070 stackexchange/bosun
```
  
Bosun should now be running on port 8070 on your docker host. OpenTSDB's interface can be accessed on port 4242.  

## Reporting Data  

In order to log data from other hosts in your environment, you will need to invoke scollector with the -h docker-server-addr:8070.  

##Updating Bosun binary  

To update the bosun binary without losing existing data, first enter the docker instance: docker exec -it <id> bash where <id> is the id or name of the docker container running bosun. Determine which bosun release you want at https://github.com/bosun-monitor/bosun/releases and copy the URL to the linux-amd64 release. From inside the docker container, run:  

```
$ apt-get update && apt-get install -y wget  
$ wget -O /bosun/bosun <bosun-release-URL>  
$ kill -HUP 1  
```
Where <bosun-release-URL> is from the github releases page. The kill -HUP 1 restarts supervisord and all its processes.
