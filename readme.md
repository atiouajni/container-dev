You don't want to use docker daemon on your mac and you have a kubernetes cluster, you can run this pod to build container images with Buildah !

**1- Run Buildah in container**
```shell
oc apply -f https://raw.githubusercontent.com/atiouajni/container-dev/main/kubernetes-manifests/pod-buildah.yaml
```

Buildah Pod need to be executed with privileged mode to access host system. Volumes are used to support overlay (check issue : https://github.com/containers/buildah/issues/867)