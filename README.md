# Déploiement WordPress sur Kubernetes

## Description
Déploiement d'une application WordPress sur un cluster Kubernetes à l'aide de manifests
YAML natifs (sans Helm), avec une base de données MySQL, dans une logique d'infrastructure
as code et de haute disponibilité.

## Architecture
- Une base de données MySQL, exposée en interne au cluster via un service ClusterIP
- Une application WordPress, connectée à MySQL et exposée à l'extérieur via un service NodePort
- Persistance des données WordPress via un volume monté sur le nœud

## Fichiers du projet
- mysql-deployment.yaml : Deployment MySQL avec 1 replica
- mysql-service.yaml : Service ClusterIP pour exposer MySQL en interne au cluster
- wordpress-deployment.yaml : Deployment WordPress avec volume monté sur /data du nœud
- wordpress-service.yaml : Service NodePort pour exposer le frontend WordPress

## Déploiement
kubectl apply -f mysql-deployment.yaml
kubectl apply -f mysql-service.yaml
kubectl apply -f wordpress-deployment.yaml
kubectl apply -f wordpress-service.yaml

## Vérification
kubectl get pods

[CAPTURE 1 ICI : kubectl get pods avec les 2 pods en Running]
kubectl get svc

[CAPTURE 2 ICI : kubectl get svc avec le service wordpress en NodePort]

## Test d'accès
curl -v http://<IP_du_pod>

Le serveur répond avec un code HTTP 302 et redirige vers /wp-admin/install.php,
ce qui confirme que WordPress est bien déployé et fonctionnel, et détecte
qu'aucune installation n'a encore été effectuée.

[CAPTURE 3 ICI : résultat du curl -v avec la redirection 302]

## Choix techniques
- Le service MySQL est nommé wordpress-mysql pour correspondre à la variable
  WORDPRESS_DB_HOST utilisée dans le deployment WordPress (résolution DNS interne
  au cluster).
- Le volume WordPress utilise un hostPath monté sur /data du nœud.
- Le service WordPress est exposé en NodePort (port 30080) pour un accès externe
  au cluster.

## Auteur
Dolly Ongmanak
