#  module 5 task-2 (Your Kubernetes cluster)

### Steps:
```bash
touch Dockerfile
docker build .
docker run -p 8080:8080 sha256:xxxxxx
docker tag da10aa926167 gcr.io/minikube-385711/demo:v2.0.0

docker images
#REPOSITORY                    TAG       IMAGE ID       CREATED       SIZE
gcr.io/minikube-385711/demo   v2.0.0    da10aa926167   6 weeks ago   4.86MB
busybox                       latest    7cfbbec8963d   6 weeks ago   4.86MB

#install Google Cloud Code extantions to vscode
gcloud auth login
gcloud config set project minikube-385711
gcloud auth configure-docker
docker push gcr.io/minikube-385711/demo:v2.0.0

gcloud container images list
#NAME
gcr.io/minikube-385711/demo



```

# dive
**A tool for exploring a docker image, layer contents, and discovering ways to shrink the size of your Docker/OCI image.**
https://github.com/wagoodman/dive

## Installation

**Ubuntu/Debian**
```bash
wget https://github.com/wagoodman/dive/releases/download/v0.9.2/dive_0.9.2_linux_amd64.deb
sudo apt install ./dive_0.9.2_linux_amd64.deb
```

**RHEL/Centos**
```bash
curl -OL https://github.com/wagoodman/dive/releases/download/v0.9.2/dive_0.9.2_linux_amd64.rpm
rpm -i dive_0.9.2_linux_amd64.rpm
```

``` bash
dive --ci --lowestEfficiency=0.9 da10aa926167

  Using default CI config
Image Source: docker://da10aa926167
Fetching image... (this can take a while for large images)
Analyzing image...
  efficiency: 100.0000 %
  wastedBytes: 0 bytes (0 B)
  userWastedPercent: NaN %
Inefficient Files:
Count  Wasted Space  File Path
None
Results:
  PASS: highestUserWastedPercent
  SKIP: highestWastedBytes: rule disabled
  PASS: lowestEfficiency
Result:PASS [Total:3] [Passed:2] [Failed:0] [Warn:0] [Skipped:1]
```

### configure minikube and deploy/demo to k8s
```bash
minikube start
k config view
k config current-context
k version --short
k get all -A
k create deploy demo --image gcr.io/minikube-385711/demo:v2.0.0
k get deploy -o wide
k get po,svc,ep --show-labels
k logs deployments/demo
k exec -it deploy/demo -- sh
k expose deployment/demo --port 80 --target-port 8080
k get po,svc,ep --show-labels
k port-forward svc/demo 8080:80&
curl localhost:8080
fg #return to background
```