# kubeleans

Demo project for running an Orleans silo in Kubernetes.

# Instructions

1. Set up your kubernetes-docker environment. There are two ways to do this:
  a. Using the [Docker Kubernetes system in the Edge version of Docker.](https://blog.docker.com/2018/01/docker-mac-kubernetes/) This is the easiest way, and I recommend it, but (as of this writing) it is only available in Docker for macOS.
  b. Using [Minikube](https://kubernetes.io/docs/getting-started-guides/minikube/).
2. Clone this repo
3. Run `make`

# What this will do
This first builds two containers: `kubeleans:silo` and `kubeleans:client`. The silo runs an Orleans silo with some simple dummy grains. The client runs a webapp that's a slightly tweaked version of the basic boilerplate that you get when you run `dotnet new reactredux`.

Next, it uses `kubectl` to deploy:
- A namespace. This is so that you can easily clean this up later by just deleting the `kubeleans` namespace.
- A NodePort service. This is what exposes the web app pod to the network
- A silo deployment. This is 3 separate identical pods that all run the silo code and participate in an Orleans cluster.
- A webapp deployment. This is a web application, exposed by the above NodePort, that will connect to the Orleans cluster and make requests to it.

Give this about 30 seconds to start up, then navigate to http://localhost:30000. You should see the friendly dotnet welcome page. Click the "Food data" link on the left and you'll see a table load by making requests to the Orleans cluster.

# Cleaning up
You can run `make clean` to delete the namespace in Kubernetes as well as removing the built images. 
