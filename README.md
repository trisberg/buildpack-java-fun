# Buildpack Sample Function

A sample function that demonstrates usage of Cloud Foundry buildpacks on
Knative Serving, using the [packs Docker images](https://github.com/sclevine/packs).

This deploys a [functions.Upper.java](https://github.com/trisberg/java-fun-upper)
sample function for riff.

## Prerequisites

### Install riff

[Install riff](https://projectriff.io/docs/getting-started-with-knative-riff-on-minikube/)

Make sure to perform the steps mentioned in "create a Kubernetes secret for pushing images to DockerHub" and "initialize the namespace" sections.

### Install buildpack build template

This sample uses the [Buildpack build template](https://github.com/knative/build-templates/blob/master/buildpack/buildpack.yaml)
in the [build-templates](https://github.com/knative/build-templates/) repo.

Install the Buildpack build template from that repo:
```shell
kubectl apply -f https://raw.githubusercontent.com/knative/build-templates/master/buildpack/buildpack.yaml
```

## Building and Running

Clone this repo (https://github.com/trisberg/buildpack-java-fun.git) and then modify `service.yaml` modifying the account in image name for the IMAGE argument
to use your DockerHub id since you can't push images to the `springdeveloper` account.

Now you can deploy this to Knative Serving from the directory of this repo using:

```shell
kubectl apply -f service.yaml
```

Once deployed, you will see that it first builds and once the build is complete the service will launch.
The status should show `Running`:

```shell
riff service list
```

Access the new service:

```shell
riff service invoke buildpack-java-fun -- -w '\n' -H 'Content-Type: text/plain' -d hello
```

## Cleaning up

To clean up the sample service:

```shell
riff service delete buildpack-java-fun
```
