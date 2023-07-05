# mediawiki
Contains docker and kubernetes files for setting up the Mediawiki v1.34 service on Kubernetes cluster
The YAML files are generated with some pre-configured values and needs to be modified for further generalization.

## Dockerfiles
* `/mediawiki-1.34/Dockerfile` - For installing and configuring Mediawiki v1.34 on a Centos7 container.
* `/mariadb-wiki/Dockerfile`  - For installing and configuring Mariadb for handling Mediawiki database.

## Kubernetes
The kubernetes objects are defined in following files: `mediawiki-deployment.yaml` `mediawiki-service.yaml` `db-deployment.yaml` `db-service.yaml` `env-configmap.yaml` `wikinetwork-networkpolicy.yaml` `secret.yaml`

* `secret.yaml` file is created for storing base64 converted password string to be used for variable MYSQL_ROOT_PASSWORD.

Replace the encrypted string by generating a new string using command: `echo -n "<new_password>" | base64`

### Deploying the Stack
On the Kubernetes workstation, run command:

```bash
kubectl apply -f mediawiki-deployment.yaml,mediawiki-service.yaml,db-deployment.yaml,db-service.yaml,env-configmap.yaml,secret.yaml
```

Once both the Mediawiki and db pods are up and running, the app UI can be accessed using the Mediawiki service hostname/ip and port.

PS C:\Users\RajKumar\Documents\Udemy Courses\Helm\work\05-07-2023> kubectl get svc
NAME         TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)          AGE
db           ClusterIP      10.0.12.77    <none>           33060/TCP        9s
kubernetes   ClusterIP      10.0.0.1      <none>           443/TCP          7h
mediawiki    LoadBalancer   10.0.227.11   20.246.225.227   8000:31580/TCP   9s



Configure the Mediawiki by providing the values as below:
* Database Host : `db:33060`
* Database Name : `wikidatabase`
* Database Username : `root`
* Database Password : `Passw0rd`

After the DB connections are setup, provide details for setting up your new Wiki.

After final submission you it will generate a LocalSettings.php file which needs to be copied on the mediawiki container at location: `/opt/rh/httpd24/root/var/www/html/mediawiki`. The Mediawiki deployment needs to be modified to change this directory location as a mount point from localhost or a S3 bucket.

# Helm Chart

Install the Helm chart using command: 

```bash
helm install  mediawiki ./helm-chart-sources/helm-chart-mediawiki-test
```
