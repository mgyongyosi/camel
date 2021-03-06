# Camel Hazelcast route on Kubernetes cluster

This quickstart run in a Java standalone container, using Spring with Apache Camel (Hazelcast component).

This quickstart is based on the Kubernetes example here: https://github.com/kubernetes/kubernetes/tree/master/examples/storage/hazelcast

This example is based on:

- Minikube (Kubernetes version >= 1.5)
- Fabric8 Maven Plugin (version >= 3.2)

First thing you'll need to do is preparing the environment.

Once your Minikube node is up and running you'll need to run the following command.
In your src/main/resources/fabric8/ folder you'll find two yaml file. Run the following command using them:

```
kubectl create -f src/main/resources/fabric8/hazelcast-service.yaml
kubectl create -f src/main/resources/fabric8/hazelcast-deployment.yaml
```

To check the correct startup of the Hazelcast instance run the following command:

```
kubectl get deployment
NAME        DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
hazelcast   1         1         1            1           1m
```

and check the status of the pod

```
kubectl get pods
NAME                                        READY     STATUS             RESTARTS   AGE
hazelcast-2400991854-1zk9g                  1/1       Running            0          29s
```

Now you can decide to scale-up your Hazelcast cluster

```
kubectl scale deployment hazelcast --replicas 4
```

and again check the status of your pods

```
kubectl get pods
NAME                                                  READY     STATUS    RESTARTS   AGE
hazelcast-4281469091-5psd3                            1/1       Running   0          4m
hazelcast-4281469091-j6p97                            1/1       Running   0          4m
hazelcast-4281469091-p9w73                            1/1       Running   0          4m
hazelcast-4281469091-xk13r                            1/1       Running   0          6m
```

You can also take a look at the logs from the pods:

```
kubectl logs hazelcast-4281469091-5psd3
2017-08-28 13:11:40.410  INFO 6 --- [           main] com.github.pires.hazelcast.Application   : Starting Application on hazelcast-4281469091-5psd3 with PID 6 (/bootstrapper.jar started by root in /)
2017-08-28 13:11:40.418  INFO 6 --- [           main] com.github.pires.hazelcast.Application   : No active profile set, falling back to default profiles: default
2017-08-28 13:11:40.517  INFO 6 --- [           main] s.c.a.AnnotationConfigApplicationContext : Refreshing org.springframework.context.annotation.AnnotationConfigApplicationContext@14514713: startup date [Mon Aug 28 13:11:40 GMT 2017]; root of context hierarchy
2017-08-28 13:11:41.235  INFO 6 --- [           main] o.s.j.e.a.AnnotationMBeanExporter        : Registering beans for JMX exposure on startup
2017-08-28 13:11:41.243  INFO 6 --- [           main] c.g.p.h.HazelcastDiscoveryController     : Asking k8s registry at https://kubernetes.default.svc.cluster.local..
2017-08-28 13:11:41.601  INFO 6 --- [           main] c.g.p.h.HazelcastDiscoveryController     : Found 3 pods running Hazelcast.
2017-08-28 13:11:41.673  INFO 6 --- [           main] c.h.instance.DefaultAddressPicker        : [LOCAL] [someGroup] [3.8.4] Interfaces is disabled, trying to pick one address from TCP-IP config addresses: [172.17.0.8, 172.17.0.7, 172.17.0.2]
2017-08-28 13:11:41.673  INFO 6 --- [           main] c.h.instance.DefaultAddressPicker        : [LOCAL] [someGroup] [3.8.4] Prefer IPv4 stack is true.
2017-08-28 13:11:41.679  INFO 6 --- [           main] c.h.instance.DefaultAddressPicker        : [LOCAL] [someGroup] [3.8.4] Picked [172.17.0.8]:5701, using socket ServerSocket[addr=/0:0:0:0:0:0:0:0,localport=5701], bind any local is true
2017-08-28 13:11:41.708  INFO 6 --- [           main] com.hazelcast.system                     : [172.17.0.8]:5701 [someGroup] [3.8.4] Hazelcast 3.8.4 (20170809 - 297a77e) starting at [172.17.0.8]:5701
2017-08-28 13:11:41.708  INFO 6 --- [           main] com.hazelcast.system                     : [172.17.0.8]:5701 [someGroup] [3.8.4] Copyright (c) 2008-2016, Hazelcast, Inc. All Rights Reserved.
2017-08-28 13:11:41.708  INFO 6 --- [           main] com.hazelcast.system                     : [172.17.0.8]:5701 [someGroup] [3.8.4] Configured Hazelcast Serialization version : 1
2017-08-28 13:11:41.982  INFO 6 --- [           main] c.h.s.i.o.impl.BackpressureRegulator     : [172.17.0.8]:5701 [someGroup] [3.8.4] Backpressure is disabled
2017-08-28 13:11:42.504  INFO 6 --- [           main] com.hazelcast.instance.Node              : [172.17.0.8]:5701 [someGroup] [3.8.4] Creating TcpIpJoiner
2017-08-28 13:11:42.635  INFO 6 --- [           main] c.h.s.i.o.impl.OperationExecutorImpl     : [172.17.0.8]:5701 [someGroup] [3.8.4] Starting 2 partition threads
2017-08-28 13:11:42.641  INFO 6 --- [           main] c.h.s.i.o.impl.OperationExecutorImpl     : [172.17.0.8]:5701 [someGroup] [3.8.4] Starting 3 generic threads (1 dedicated for priority tasks)
2017-08-28 13:11:42.646  INFO 6 --- [           main] com.hazelcast.core.LifecycleService      : [172.17.0.8]:5701 [someGroup] [3.8.4] [172.17.0.8]:5701 is STARTING
2017-08-28 13:11:42.678  INFO 6 --- [cached.thread-2] c.hazelcast.nio.tcp.InitConnectionTask   : [172.17.0.8]:5701 [someGroup] [3.8.4] Connecting to /172.17.0.7:5701, timeout: 0, bind-any: true
2017-08-28 13:11:42.678  INFO 6 --- [cached.thread-3] c.hazelcast.nio.tcp.InitConnectionTask   : [172.17.0.8]:5701 [someGroup] [3.8.4] Connecting to /172.17.0.2:5701, timeout: 0, bind-any: true
2017-08-28 13:11:42.686  INFO 6 --- [cached.thread-3] c.h.nio.tcp.TcpIpConnectionManager       : [172.17.0.8]:5701 [someGroup] [3.8.4] Established socket connection between /172.17.0.8:49353 and /172.17.0.2:5701
2017-08-28 13:11:42.688  INFO 6 --- [cached.thread-2] c.h.nio.tcp.TcpIpConnectionManager       : [172.17.0.8]:5701 [someGroup] [3.8.4] Established socket connection between /172.17.0.8:59023 and /172.17.0.7:5701
2017-08-28 13:11:48.820  INFO 6 --- [ration.thread-1] com.hazelcast.system                     : [172.17.0.8]:5701 [someGroup] [3.8.4] Cluster version set to 3.8
2017-08-28 13:11:48.832  INFO 6 --- [ration.thread-1] c.h.internal.cluster.ClusterService      : [172.17.0.8]:5701 [someGroup] [3.8.4] 

Members [3] {
	Member [172.17.0.2]:5701 - 3625b0b6-9939-4188-8323-3e1f1f74b2e0
	Member [172.17.0.7]:5701 - f7698972-1743-4d63-8e3f-3391d41882f9
	Member [172.17.0.8]:5701 - 20072d52-7525-46a2-8cf2-226ad2fd4540 this
}

2017-08-28 13:11:49.436  INFO 6 --- [thread-Acceptor] c.h.nio.tcp.SocketAcceptorThread         : [172.17.0.8]:5701 [someGroup] [3.8.4] Accepting socket connection from /172.17.0.6:36531
2017-08-28 13:11:49.439  INFO 6 --- [cached.thread-3] c.h.nio.tcp.TcpIpConnectionManager       : [172.17.0.8]:5701 [someGroup] [3.8.4] Established socket connection between /172.17.0.8:5701 and /172.17.0.6:36531
2017-08-28 13:11:50.719  INFO 6 --- [           main] com.hazelcast.core.LifecycleService      : [172.17.0.8]:5701 [someGroup] [3.8.4] [172.17.0.8]:5701 is STARTED
2017-08-28 13:11:50.774  INFO 6 --- [           main] com.github.pires.hazelcast.Application   : Started Application in 11.009 seconds (JVM running for 11.45)
2017-08-28 13:11:56.443  INFO 6 --- [ration.thread-0] c.h.internal.cluster.ClusterService      : [172.17.0.8]:5701 [someGroup] [3.8.4] 

Members [4] {
	Member [172.17.0.2]:5701 - 3625b0b6-9939-4188-8323-3e1f1f74b2e0
	Member [172.17.0.7]:5701 - f7698972-1743-4d63-8e3f-3391d41882f9
	Member [172.17.0.8]:5701 - 20072d52-7525-46a2-8cf2-226ad2fd4540 this
	Member [172.17.0.6]:5701 - 67881b8b-04a2-4888-8283-f9e39f280282
}


```

### Building and running

Navigate to the project folder and the example can be built with

    mvn clean -Pkubernetes-install fabric8:deploy

When the example runs in fabric8, you can use the Kubectl command tool to inspect the status

To list all the running pods:

    kubectl get pods

Then find the name of the pod that runs this quickstart, and output the logs from the running pods with:

    kubectl logs <name of pod>

and you should see something like this:

```
INFO: hz.client_0 [someGroup] [3.8.2] HazelcastClient 3.8.2 (20170518 - a60f944) is CLIENT_CONNECTED
2017-05-26 12:14:57,135 [main           ] INFO  SpringCamelContext             - Apache Camel 2.20.0-SNAPSHOT (CamelContext: camel-1) is starting
2017-05-26 12:14:57,136 [main           ] INFO  ManagedManagementStrategy      - JMX is enabled
2017-05-26 12:14:57,271 [main           ] INFO  DefaultTypeConverter           - Loaded 193 type converters
2017-05-26 12:14:57,302 [main           ] INFO  DefaultRuntimeEndpointRegistry - Runtime endpoint registry is in extended mode gathering usage statistics of all incoming and outgoing endpoints (cache limit: 1000)
2017-05-26 12:14:57,405 [main           ] INFO  SpringCamelContext             - StreamCaching is not in use. If using streams then its recommended to enable stream caching. See more details at http://camel.apache.org/stream-caching.html
May 26, 2017 12:14:57 PM com.hazelcast.client.connection.ClientConnectionManager
INFO: hz.client_0 [someGroup] [3.8.2] Authenticated with server [172.17.0.7]:5701, server version:3.8.2 Local address: /172.17.0.8:37538
May 26, 2017 12:14:58 PM com.hazelcast.client.connection.ClientConnectionManager
INFO: hz.client_0 [someGroup] [3.8.2] Authenticated with server [172.17.0.2]:5701, server version:3.8.2 Local address: /172.17.0.8:59990
May 26, 2017 12:14:58 PM com.hazelcast.client.connection.ClientConnectionManager
INFO: hz.client_0 [someGroup] [3.8.2] Authenticated with server [172.17.0.4]:5701, server version:3.8.2 Local address: /172.17.0.8:38928
2017-05-26 12:14:59,956 [main           ] INFO  SpringCamelContext             - Route: route1 started and consuming from: timer://foo?period=5000
2017-05-26 12:14:59,960 [main           ] INFO  SpringCamelContext             - Route: route2 started and consuming from: hazelcast-topic://foo
2017-05-26 12:14:59,961 [main           ] INFO  SpringCamelContext             - Total 2 routes, of which 2 are started.
2017-05-26 12:14:59,963 [main           ] INFO  SpringCamelContext             - Apache Camel 2.20.0-SNAPSHOT (CamelContext: camel-1) started in 2.825 seconds
2017-05-26 12:15:00,972 [0 - timer://foo] INFO  route1                         - Producer side: Sending data to Hazelcast topic..
2017-05-26 12:15:01,001 [lient_0.event-3] INFO  route2                         - Consumer side: Detected following action: received
2017-05-26 12:15:05,967 [0 - timer://foo] INFO  route1                         - Producer side: Sending data to Hazelcast topic..
2017-05-26 12:15:05,974 [lient_0.event-3] INFO  route2                         - Consumer side: Detected following action: received
2017-05-26 12:15:10,961 [0 - timer://foo] INFO  route1                         - Producer side: Sending data to Hazelcast topic..
2017-05-26 12:15:10,963 [lient_0.event-3] INFO  route2                         - Consumer side: Detected following action: received
2017-05-26 12:15:15,961 [0 - timer://foo] INFO  route1                         - Producer side: Sending data to Hazelcast topic..
2017-05-26 12:15:15,963 [lient_0.event-3] INFO  route2                         - Consumer side: Detected following action: received
2017-05-26 12:15:20,962 [0 - timer://foo] INFO  route1                         - Producer side: Sending data to Hazelcast topic..
2017-05-26 12:15:20,964 [lient_0.event-3] INFO  route2                         - Consumer side: Detected following action: received
2017-05-26 12:15:25,962 [0 - timer://foo] INFO  route1                         - Producer side: Sending data to Hazelcast topic..
2017-05-26 12:15:25,963 [lient_0.event-3] INFO  route2                         - Consumer side: Detected following action: received
2017-05-26 12:15:30,962 [0 - timer://foo] INFO  route1                         - Producer side: Sending data to Hazelcast topic..
2017-05-26 12:15:30,965 [lient_0.event-3] INFO  route2                         - Consumer side: Detected following action: received
2017-05-26 12:15:35,961 [0 - timer://foo] INFO  route1                         - Producer side: Sending data to Hazelcast topic..
2017-05-26 12:15:35,963 [lient_0.event-3] INFO  route2                         - Consumer side: Detected following action: received
2017-05-26 12:15:40,962 [0 - timer://foo] INFO  route1                         - Producer side: Sending data to Hazelcast topic..
2017-05-26 12:15:40,964 [lient_0.event-3] INFO  route2                         - Consumer side: Detected following action: received

```
