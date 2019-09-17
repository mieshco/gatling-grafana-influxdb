# Gatling Real-time Monitoring

Gatling can provide live metrics via the Graphite protocol which can be persisted and visualised.
This project shows how to use InfluxDB, Graphite, and Grafana to monitor Gatling tests in real-time.

Use `docker-compose up` to launch Grafana and Influx DB infrastructure. Go to [localhost:3000](http://localhost:3000) to see Grafana dashboard.

Configure your Gatling instance to use graphite protocol to send data to InfluxDB. Update following config in data section of your gatling.conf file:
```
data {
  writers = [console, file, graphite]      # The list of DataWriters to which Gatling write simulation data (currently supported : console, file, graphite, jdbc)
  .....
  graphite {
      light = false              # only send the all* stats
      host = "localhost"         # The host where the Carbon server is located
      port = 2003                # The port to which the Carbon server listens to (2003 is default for plaintext, 2004 is default for pickle)
      protocol = "tcp"           # The protocol used to send data to Carbon (currently supported : "tcp", "udp")
      #rootPathPrefix = "gatling" # The common prefix of all metrics sent to Graphite
      bufferSize = 8192          # Internal data buffer size, in bytes
      writePeriod = 1            # Write period, in seconds
    }
  .....
}
```
Fore more details see Gatling docs [here.](https://gatling.io/docs/current/realtime_monitoring/?highlight=graphite)


You can see a sample Grafana dashboard below.

<p align="left">
<img src="screenshots/grafana.png" width="500"/>
</p>

WIP: steps to install on raw UBUNTU
sudo apt-get update
sudo apt-get install git
git clone https://github.com/marufaytekin/gatling-grafana-influxdb.git
cd gatling-grafana-influxdb/
nano docker-compose.yml
version: '3.2'

services:
influxdb:
build: influxdb
env_file: configuration.env
ports:
- '8086:8086'
- '2003:2003'

grafana:
build: grafana
env_file: configuration.env
links:
- influxdb
ports:
      - '3000:3000'
   volumes:
      - /etc/grafana/

sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce
  or docker on debian
sudo apt-get install docker-compose
sudo usermod -aG docker $USER
sudo docker run hello-world
sudo docker-compose up

  421  docker stop $(docker ps -a -q)
  
  428  sudo chmod +x /usr/local/bin/docker-compose
docker-compose up -d
  431  docker run -it -p 8086:8086 influxdb
  432  docker ps
  433  docker run -it -p 8086:8086 gatlinggrafanainfluxdb_influxdb
  434  curl -i -XPOST http://localhost:8086/query 8 --data-urlencode “q=SHOW DATABASES”
