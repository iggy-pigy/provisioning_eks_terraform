# jenkins_helm

## Description

## Instructions

The following instructions assume you have a Kubernetes cluster provisioned that you can access via `kubectl`

They also assume that you have installed Helm locally.

**Add the stable Helm repository**

helm repo add stable https://kubernetes-charts.storage.googleapis.com/

**Simply install a default Jenkins installation using Helm**

```
helm install jenkins-app -f values.yaml stable/jenkins
```

You should get something like:

```
NAME: jenkins-1586894313
LAST DEPLOYED: Tue Apr 14 20:58:36 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
1. Get your 'admin' user password by running:
  printf $(kubectl get secret --namespace default jenkins-app -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
2. Get the Jenkins URL to visit by running these commands in the same shell:
  NOTE: It may take a few minutes for the LoadBalancer IP to be available.
        You can watch the status of by running 'kubectl get svc --namespace default -w jenkins-1586894313'
  export SERVICE_IP=$(kubectl get svc --namespace default jenkins-1586894313 --template "{{ range (index .status.loadBalancer.ingress 0) }}{{ . }}{{ end }}")
  echo http://$SERVICE_IP:80/login

3. Login with the password from step 1 and the username: admin


For more information on running Jenkins on Kubernetes, visit:
https://cloud.google.com/solutions/jenkins-on-container-engine
```

**Wait for the application to fully start**

```
kubectl get pods --watch
```

Once you see READY 1/1 you can cancel the watch with `Ctrl+C`

**Get your Jenkins admin password**

The output of the deployment will have shown a command for getting your Jenkins admin password.

It'll look similar to the one below. Run that command and it will print your default admin password.

```
printf $(kubectl get secret --namespace default jenkins-app -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
```

**Get your DNS for the Jenkins app**

```
kubectl get services
```

The item listed under __EXTERNAL-IP__ is your DNS address.

Open up your browser and go to that address.

Whack in your username of __admin__ and the password from the previous step.

And there you have it a fully functioning Jenkins app 

