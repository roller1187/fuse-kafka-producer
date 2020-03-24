<img src="https://developers.redhat.com/blog/wp-content/uploads/2018/10/Untitled-drawing-4.png" data-canonical-src="https://developers.redhat.com/blog/wp-content/uploads/2018/10/Untitled-drawing-4.png" width="300" height="140" />

# Acrostic Fuse Kafka Producer

The purpose of this service is to:
  1. Poll XML static service to obtain acrostic message
  2. Write the XML message into a Kafka topic to integrate with modern applications running in OpenShift

---

## Instructions for deploying on OpenShift:
  1. Login to OpenShift:
```sh
oc login <openshift_cluster>
```
  2. Use existing Kafka project created during the deployment of the [kafka-consumer](https://github.com/roller1187/kafka-consumer) service:
```sh
oc project kafka-$(oc whoami)
```
  3. Deploy the service using s2i (Source-2-Image). Don't forget to provide a Kafka topic:
```sh
oc new-app redhat-openjdk18-openshift:1.4~https://github.com/roller1187/fuse-kafka-producer.git \
    --env KAFKA_BACKEND_TOPIC=my-topic \
    --env SPRING_KAFKA_BOOTSTRAP_SERVERS=my-cluster-kafka-external-bootstrap.kafka-demo.svc.cluster.local:9094
```
  4. Add ConfigMap to Fuse Producer:
```sh
oc set volume dc/fuse-kafka-producer --add --type=configmap --configmap-name=kafka-cert --mount-path=/tmp/certs
```

*Acrostic example:

![Acrostic](https://www.researchgate.net/profile/Andrew_Finch/publication/260593143/figure/fig3/AS:392472879484941@1470584234596/Acrostic-poem-Teaching-points-Spelling-Vocabulary-Dictionary-Holmes-Moulton-2001.png)

