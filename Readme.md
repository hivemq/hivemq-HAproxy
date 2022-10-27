# Welcome

Welcome to this technology demonstrator where we setup a 3 node HiveMQ cluster along with a HA proxy that acts as a load balancer and single IP entrypoint to this cluster. 

![Alt text](images/HAproxy_graphics_v1.png?raw=true "scenaria")



Start by cloning this repo to your docker and docker compose enabled platform and CD into it. Then set the HiveMQ version that you want to deploy and build the local HiveMQ image. This image will include the HiveMQ config.xlm file that enables the cluster:

```
<cluster>
    <enabled>true</enabled>
    <transport>
        <tcp>
            <!-- replace this IP with the IP address of your interface -->
        </tcp>
    </transport>
    <discovery>
        <broadcast>
            <!-- replace this IP with the broacast IP address of your subnet -->
        </broadcast>
    </discovery>
</cluster>
```

Please refer to the following commands:


```
git clone https://github.com/hivemq/hivemq-HAproxy.git
cd hivemq-HAproxy
export HIVEMQ_VERSION=4.9.0
sudo chmod +x build.sh
./build.sh
docker-compose up
```
# checking your cluster

If you run a `docker ps` the output will show 3 HiveMQ nodes running along with one HAproxy instance:

![Alt text](images/Docker-PS-all.png?raw=true "LB all Up!")

Checkout: http://0.0.0.0:8404/stats 
Are all nodes detected by the loadbalancer ?

![Alt text](images/LB-allup.png?raw=true "LB all Up!")

Checkout: http://0.0.0.0:8080 
Are all nodes running and formed a cluster as shown in the HiveMQ Control center (default password: `admin/hivemq`) ?

![Alt text](images/CC-allup.png?raw=true "LB all Up!")

HiveMQ CLI tools
https://www.hivemq.com/blog/mqtt-cli/

Test with :
`Mqtt sub -h 0.0.0.0   -t "testtopic"` in a seperate CLI window
and 
`Mqtt pub -h 0.0.0.0 -m "test" -t "testtopic"`

# Malfunction simulation

![Alt text](images/Docker-PS-all.png?raw=true "LB all Up!")

You can simulate malfunction of one of the nodes by simply killing it: 
`docker kill a21ec7267a6d`

Now the node has been gone wich is shows in HiveMQ CC and in the HAproxy stats screen:

![Alt text](images/CC-2nodeup.png?raw=true "CC just two Up!")

![Alt text](images/LB-2nodeup.png?raw=true "LB just two Up!")


You can restore the original 3 node configuration simply by re-running `docker-compose up`.

Please clean, up afterwards with `docker-compose down`.

### More info ?

For contact information please reach out to kamiel.straatman@hivemq.com

### ToDo
extend this demo to include a roling version upgrade.
