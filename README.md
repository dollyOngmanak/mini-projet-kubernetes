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

<img width="1200" height="127" alt="MyScreen7_13_2026_12_51_32_PM" src="https://github.com/user-attachments/assets/4c4b20cc-4220-439b-9707-24262862dd11" />

kubectl get svc

<img width="1200" height="154" alt="MyScreen7_13_2026_12_52_59_PM" src="https://github.com/user-attachments/assets/ddd583b7-2b54-4c08-aea5-e73a8690ba33" />


## Test d'accès
curl -v http://10.244.0.5

Le serveur répond avec un code HTTP 302 et redirige vers /wp-admin/install.php,
ce qui confirme que WordPress est bien déployé et fonctionnel, et détecte
qu'aucune installation n'a encore été effectuée.

<img width="1200" height="314" alt="MyScreen7_13_2026_12_55_22_PM" src="https://github.com/user-attachments/assets/4e319c22-1d61-4016-921d-722af67aec2e" />
<img width="1200" height="324" alt="MyScreen7_13_2026_12_56_29_PM" src="https://github.com/user-attachments/assets/d923dae7-85ed-424e-a34d-a3e8fffca52c" />
<img width="1200" height="598" alt="MyScreen7_13_2026_1_02_32_PM" src="https://github.com/user-attachments/assets/41357b0b-24ce-4c99-81e4-7c3ff34927ef" />




## Choix techniques
- Le service MySQL est nommé wordpress-mysql pour correspondre à la variable
  WORDPRESS_DB_HOST utilisée dans le deployment WordPress (résolution DNS interne
  au cluster).
- Le volume WordPress utilise un hostPath monté sur /data du nœud.
- Le service WordPress est exposé en NodePort (port 30080) pour un accès externe
  au cluster.

## Auteur
Dolly Ongmanak
