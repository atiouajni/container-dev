**1- Run Buildah in container**

You don't want to use docker daemon on your mac and you have a kubernetes cluster, you can run this pod to build container images with Buildah !

```shell
oc apply -f https://raw.githubusercontent.com/atiouajni/container-dev/main/kubernetes-manifests/pod-buildah.yaml
```

Buildah Pod need to be executed with privileged mode to access host system. Volumes are used to support overlay --check the issue [here](https://github.com/containers/buildah/issues/867)

**2- Build an image with CEKit & Buildah**
[CEKit](https://docs.cekit.io/en/latest/index.html) helps to build container images from image definition files with strong focus on modularity and code reuse. 

```shell
#build the image
buildah bud -f ./Dockerfile-buildah-cekit -t cekit-buildah .

#get image id
buildah images

# push image to registry
buildah login registry.hub.docker.com
buildah push <image-id> docker://registry.hub.docker.com/atiouajni/cekit-buildah:latest
```

**3-Run CEKit and Buildah in container**

```shell
oc apply -f https://raw.githubusercontent.com/atiouajni/container-dev/main/kubernetes-manifests/pod-cekit-buildah.yaml
```
 
