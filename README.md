# Kubernetes Project for Ynov - Groupe 4

Ce projet a été créer dans le but de fonctionner sur un Cluster Kubernetes. Ce cluster pourra être installer sur un cloud provider comme sur un cluster kube local.

## Outils

Use the package manager [pip](https://pip.pypa.io/en/stable/) to install foobar.

```bash
pip install foobar
```

## Pour tester

Pour tester le cluster kube sur sa machine en local, veillez à avoir installer un minikube (ou equivalent) sur votre machine.
Puis, après avoir cloner le repo, il faudra juste modifier la valeur du service web afin de ne plus le mettre en ClusterIP mais en LoadBalancer afin qu'il ai sa propre External IP.

```bash
# WEB Service
apiVersion: v1  
kind: Service
metadata:
  name: web-service
  namespace: projet
spec:
  selector:
    app: webmvc 
  ports:
    - name: webmc-port
      port: 80
      targetPort: 80
  type: LoadBalancer
```
Il ne reste plus qu'à lancer la création, sans lancer l'Ingress

```bash
kubectl create -f allinone.yml
```

## Installation de l'application

Il va falloir installer helm sur le cluster Kube
https://helm.sh/docs/intro/install/

Puis nous allons ajouter un controller d'ingress, pour cela, nous allons utiliser Nginx
On commence par créer son propre namespace

```bash
kubectl create namespace ingress-projet
```

On ajoute le repo ingress-nginx et on le met à jour 

```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
```
Puis on install le controller Ingress 

```bash
helm install nginx-ingress ingress-nginx/ingress-nginx --namespace ingress-projet
```

Puis nous pouvons creer le projet puis l'ingress

```bash
kubectl create -f allione.yml
kubectl create -f ingress.yml
```

## Installation du monitoring du cluster

Pour la solution de monitoring des metrics avec Prometheus, nous avons fait le choix d'utiliser le helm chart de prometheus officiel, puis nous override les parametres grâce au fichier prometheus/values.yml

Pour commencer, on créer un nouveau namespace monitoring

```bash
kubectl create namespace monitoring
```

Puis on ajoute le repo helm et on l'update

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

Puis on ajoute le repo helm et on l'update

```bash
helm install prometheus prometheus-community/kube-prometheus-stack -f prometheus/values.yml --namespace monitoring
```

## Usage

```python
import foobar

foobar.pluralize('word') # returns 'words'
foobar.pluralize('goose') # returns 'geese'
foobar.singularize('phenomena') # returns 'phenomenon'
```

## Commandes utiles
Se connecter au Cluster azure

```bash
az aks get-credentials --resource-group groupe4 --name clus4
```

Changer de context

```bash
kubectl config get-contexts
kubectl config use-context
```
## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT](https://choosealicense.com/licenses/mit/)