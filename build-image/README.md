# Learning Software for Open Networking in the Cloud (SONiC) 
https://github.com/sonic-net/sonic-buildimage

## Images
https://sonic.software/
Broadcom virtual switch, version 3.1.2
Account: admin/YourPaSsWoRd
Interface: format with file sonicsw.yml
> /opt/unetlab/html/templates/intel/sonicsw.yml

docker pull quanghong/sonic-rest-api-build-image:latest
sudo docker run -d \
  --name restapi \
  --network host \
  --restart always \
  -v /etc/sonic:/etc/sonic \
  quanghong/sonic-rest-api-build-image:latest

docker run -d --rm -p8090:8090 -p6379:6379 --name rest-api --cap-add NET_ADMIN --privileged -t quanghong/sonic-rest-api-build-image:latest

docker run -d --rm -p8090:8090 --name rest-api --cap-add NET_ADMIN --privileged -t quanghong/sonic-rest-api-build-image:latest

curl -u admin:YourPaSsWoRd http://localhost:8090/restconf/data/openconfig-system:system/config

curl get -u admin:YourPaSsWoRd http://localhost:6379/restconf/data/openconfig-system:system/config

curl http://localhost:6379/
