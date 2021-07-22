
**1- Run Buildah in container**

You don't want to use docker daemon on your mac and you have a kubernetes cluster, you can run this pod to build container images with Buildah !

```shell
oc apply -f ./kubernetes-manifests/pod-buildah.yaml
```

Buildah Pod need to be executed with privileged mode to access host system. Volumes are used to support overlay --check the issue [here](https://github.com/containers/buildah/issues/867)

**2- Build an image with CEKit & Buildah**
[CEKit](https://docs.cekit.io/en/latest/index.html) helps to build container images from image definition files with strong focus on modularity and code reuse. 

```shell
git clone https://github.com/atiouajni/container-dev; cd container-dev

#if you are using buildah in container, perform this task
oc rsync . buildah:/tmp; oc exec -it buildah -- /bin/sh
cd /tmp

#performs the build of the new image defined by Dockerfile
buildah bud -f ./Dockerfile.buildah-cekit -t cekit-buildah .

#push image to docker registry
buildah login registry.hub.docker.com
buildah push cekit-buildah docker://registry.hub.docker.com/atiouajni/cekit-buildah:latest
```

**3- Build an image with CEKit & Buildah using BuildConfig**
```shell
git clone https://github.com/atiouajni/container-dev; cd container-dev
cat Dockerfile.buildah-cekit| oc new-build --name=cekit-buildah-builder --to=cekit-buildah --dockerfile='-' 
```

**4-Run CEKit and Buildah in container**

```shell
oc apply -f https://raw.githubusercontent.com/atiouajni/container-dev/main/kubernetes-manifests/pod-cekit-buildah.yaml
```
 
