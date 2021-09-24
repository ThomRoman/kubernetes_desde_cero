# Presentación: 
[Presentación](https://1drv.ms/p/s!AoX_zvfKf0RXj8g35ppcFemsnZdOew?e=yT8mrP "Presentación")

Origen de Kubernetes: https://kube.academy/courses/getting-started/lessons/the-origin-of-kubernetes 

Borg: https://kubernetes.io/blog/2015/04/borg-predecessor-to-kubernetes/ 

Kubernetes wiki: https://en.wikipedia.org/wiki/Kubernetes 

CNCF (Cloud Native Computing Foundation): https://www.cncf.io/projects/ 

Berkeley Software Distribution: https://en.wikipedia.org/wiki/Berkeley_Software_Distribution 

CHROOT: https://en.wikipedia.org/wiki/Chroot 

FreeBSD: https://en.wikipedia.org/wiki/FreeBSD

FreeBSD jail: https://en.wikipedia.org/wiki/FreeBSD_jail

Join us in celebrating 30 years of Linux: https://linuxfoundation.org/linux30th/ 

Linux namespaces: https://en.wikipedia.org/wiki/Linux_namespaces

cgroups: https://en.wikipedia.org/wiki/Cgroups 

Solaris Containers: https://en.wikipedia.org/wiki/Solaris_Containers

LXC: https://en.wikipedia.org/wiki/LXC 

lmctfy: https://en.wikipedia.org/wiki/Lmctfy / https://github.com/google/lmctfy 

Docker: https://en.wikipedia.org/wiki/Docker_(software)

APPC: https://github.com/appc/spec

OCI: https://opencontainers.org/

kata Containers: https://katacontainers.io/learn/

gVisor: https://github.com/google/gvisor

Podman: https://podman.io/ 

Containerd: https://www.cncf.io/projects/containerd/


Manifiesto para el deploymento del punto 1.4: Azure Vote App 

apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-back
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      nodeSelector:
        "kubernetes.io/os": linux
      containers:
      - name: azure-vote-back
        image: mcr.microsoft.com/oss/bitnami/redis:6.0.8
        env:
        - name: ALLOW_EMPTY_PASSWORD
          value: "yes"
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-front
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      nodeSelector:
        "kubernetes.io/os": linux
      containers:
      - name: azure-vote-front
        image: mcr.microsoft.com/azuredocs/azure-vote-front:v1
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front


    
    
