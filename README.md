# This is how you get the apps in your MicroK8s cluster to respond to domain names on the interwebs.

Apart from enabling Istio, Ingress, DNS and MetalLB on your MicroK8s cluster, you need to setup a service, istio gateway and vistual service from every app. The examples here also show how to up different routes for the apps.

The Bookinfo example was taken from the Istio website and the Hello World was an experiment to proof the concept and get an app working with my own domain name.

In my case, the MicroK8s cluster sits on vms in my server behind a PFSense router. So the domain name was configured to use my wan address, which is redirected by PFSense into the ip shown by the following command:
``` 
    $ kubectl get svc istio-ingressgateway -n istio-system 
```

If you get nothing or a _"forever pending"_ external ip, you need to enable **MetalLB**. 
MicroK8s offers MetalLB as a quick and dirty solution to the external ip problems on self-hosted clusters.
When enabling MetalLb, you will need to assign an ip range. I set it up with a range within the same v-lan as the vm running my cluster.

---
To test with these apps, simply: 
``` 
    $ kubectl apply -f hello-world.yaml 
``` 
or
``` 
    $ kubectl apply -f bookinfo.yaml
    $ kubectl apply -f bookinfo-gateway.yaml 
``` 