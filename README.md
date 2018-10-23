# StatefulSet-RabbitMQ-Kubernetes
2 replicas RabbitMQ runs on host with local mount, with stable DNS names using peer discovery plugin

This YAML file contains:
* NodePort service to expose the amqp protocol + management protocol
* Headless service to provide each pod a stable DNS name
* ConfigMap to change some configurations (and also set the peer discovery configuration)
* 2 Persistent volumes - one for each replica (as currently local volume doesn't support dynamic provisioning), each one refers to a local disk on the host, bounded to that specific host (it's name is ubuntukuber, change it to your node name).
** Basically, we could create a multi host cluster, but we'll have to bound pods to be scheduled on the specific host (to find the host filesystem) - this was not tested by me. (it's all about nodeAffinity)
* StatefulSet that besides mapping the config directory, it maps the data folder of the rabbit MQ to our volume.
* RABBITMQ_NODENAME is set to the fully qualified cluster dns name (that the headless service provides)

Base YAML was taken from https://github.com/rabbitmq/rabbitmq-peer-discovery-k8s/blob/master/examples/k8s_statefulsets/rabbitmq_statefulsets.yaml and was expanded to host names + the persistent local storage
