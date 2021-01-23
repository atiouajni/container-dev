**1- Run Buildah in container**

You don't want to use docker daemon on your mac and you have a kubernetes cluster, you can run this pod to build container images with Buildah !

```shell
oc apply -f https://raw.githubusercontent.com/atiouajni/container-dev/main/kubernetes-manifests/pod-buildah.yaml
```

Buildah Pod need to be executed with privileged mode to access host system. Volumes are used to support overlay --check the issue [here](https://github.com/containers/buildah/issues/867)

**2- Build an image with CEKit & Buildah**
[CEKit](https://docs.cekit.io/en/latest/index.html) helps to build container images from image definition files with strong focus on modularity and code reuse. 

```shell
#performs the build of the new image defined by Dockerfile
buildah bud -f ./Dockerfile.buildah-cekit -t cekit-buildah .

#get image id
buildah images

#push image to docker registry
buildah login registry.hub.docker.com
buildah push localhost/cekit-buildah docker://registry.hub.docker.com/atiouajni/cekit-buildah:latest

#push to openshift internal registry
buildah login --tls-verify=false -u anyone -p $(cat /var/run/secrets/kubernetes.io/serviceaccount/token) image-registry.openshift-image-registry.svc:5000
buildah push --tls-verify=false localhost/cekit-buildah docker://image-registry.openshift-image-registry.svc:5000/openshift/cekit-buildah
```

If you are runnning Buildah in container, you should clone this repository in your host then rsyn content to the pod
```shell
git clone https://github.com/atiouajni/container-dev; cd container-dev
oc get pod/buildah
oc rsync . buildah:/tmp/container-dev/ 
```

**3-Run CEKit and Buildah in container**

```shell
oc apply -f https://raw.githubusercontent.com/atiouajni/container-dev/main/kubernetes-manifests/pod-cekit-buildah.yaml
```
 
