Installing Kubeflow pipeline(locally) in the Windows system

Installing Kubeflow Pipeline (KFP) on Windows can be a bit challenging since KFP is primarily designed to run on Linux-based systems. However, you can set up a Windows-based development environment and run KFP using a Docker container.

The following steps will help you to install Kubeflow pipelines for windows system

**Step 1: Install Docker Desktop**
Docker Desktop: https://docs.docker.com/desktop/install/windows-install/

**Step 2: Install Minikube**
Minikube installation link: https://minikube.sigs.k8s.io/docs/start/

Open up your PowerShell in Administrator mode and type in this command
Download and run the installer for the latest release
```
New-Item -Path ‘c:\’ -Name ‘minikube’ -ItemType Directory -Force
```

```
Invoke-WebRequest -OutFile ‘c:\minikube\minikube.exe’ -Uri ‘https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing
```

Add the minikube.exe binary to your PATH.
Make sure to run PowerShell as Administrator.
```
$oldPath = [Environment]::GetEnvironmentVariable(‘Path’, [EnvironmentVariableTarget]::Machine)
if ($oldPath.Split(‘;’) -inotcontains ‘C:\minikube’){ `
[Environment]::SetEnvironmentVariable(‘Path’, $(‘{0};C:\minikube’ -f $oldPath), [EnvironmentVariableTarget]::Machine) `
}
```

**Step 3: Install K8s**
Kubectl commands
https://www.kubeflow.org/docs/components/pipelines/v1/installation/localcluster-deployment/

To deploy the Kubeflow Pipelines, run the following commands:
```
Set PIPELINE_VERSION=1.8.5

kubectl apply -k “github.com/kubeflow/pipelines/manifests/kustomize/cluster-scoped-resources?ref=$PIPELINE_VERSION”

kubectl wait — for condition=established — timeout=60s crd/applications.app.k8s.io

kubectl apply -k “github.com/kubeflow/pipelines/manifests/kustomize/env/platform-agnostic-pns?ref=$PIPELINE_VERSION”
```

Verify that the Kubeflow Pipelines UI is accessible by port-forwarding:

```
kubectl port-forward -n kubeflow svc/ml-pipeline-ui 8080:80
```