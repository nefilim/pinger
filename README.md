# Pinger

A simple network latency and packet loss monitor, with beautiful graphs in Grafana!

![in action](https://s3-us-west-2.amazonaws.com/pinger-static/pinger.png#2)

### Installation

1. Clone this repo locally where you have space to host the data gathered (one GB should be plenty!) 
1. [Install Docker 17.12.0+ (preferably 18.02.0+)](https://docs.docker.com/install/)
1. [Install the latest Docker Compose](https://docs.docker.com/compose/install/)
1. Create directories for data for storage (in the cloned directory): 
  * `mkdir grafana-data prometheus-data`
  * `chown 472 grafana-data`
  
### Configuration

1. Modify `etc/application.conf` to include the hosts you want to monitor
1. Start all the services with: `docker-compose up -d` (it will start containers for Pinger, Grafana, Prometheus)
1. Check the Pinger logs to make sure your configuration were picked up: `docker-compose logs pinger`
 
You want to see something like:

> pinger        | 23:34:33.803 [main] INFO  o.n.p.Main$ - Creating PingController for host [4.2.2.2] in network [public] 

reflecting your configuration.

Your Grafana & Prometheus data will be stored on your docker host in `./grafana-data` and `prometheus-data`. This should allow you to preserve your dashboard configurations and data while upgrading or recreating the Grafana & Prometheus containers.

### Setting up Grafana

1. Login to the Grafana console at http://your_docker_host:3000 with the default credentials, username: admin password: admin
1. Change the admin credentials or click Skip
1. Click on `Create your first data source`
1. Select Prometheus
1. In the HTTP/Access section enter: `http://prometheus:9090/`
1. Click `Save & Test` - it should show a green success box "Data source is working"
1. Click on the four squares in the left hand column to go back to the Home Dashboard
1. Click on `Create your first dashboard`
1. In the New Panel pane, select Graph
1. Hover over the graph and press `e` to edit it
1. In the Metrics tab for Series A enter the following prometheus query: `average_latency{network="internal"}`
1. In the Legend format box below enter: `{{host}}`
1. In the General Tab enter a suitable title `Internal Latency`
1. Repeat this process for additional networks and for packet loss (`average_packetloss{network="X"}`) - I like to set the Y axis minimum to 0 for packet loss
1. **IMPORTANT:** save your new dashboard with Cmd+S / Ctrl+S 



 
