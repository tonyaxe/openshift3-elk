# ELK-Stack on Openshift v3

These are modified dockerfiles from the official docker images. I removed the gosu stuff and made sure relevant files are world readable.

## Usage
There is a docker-compose and a openshift template file in the `example` directory.
### Local
```
docker run -it --rm --name elasticsearch lbischof/elasticsearch
docker run -it --rm --link elasticsearch:elasticsearch -p 5601:5601 lbischof/kibana
s2i build https://github.com/lbischof/openshift3-elk.git lbischof/logstash logstash --context-dir=example
docker run -it --rm --link elasticsearch:elasticsearch logstash
```
### Openshift
```
oc new-project elk
oc new-app lbischof/kibana
oc new-app lbischof/elasticsearch
oc new-app lbischof/logstash~https://github.com/lbischof/openshift3-elk.git --context-dir example --name logstash-git
oc expose service kibana
```

