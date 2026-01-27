# Déploiement Bitwarden - INF3611

## Étudiant
- **Nom:** MAKUNE KOUGOUM BIJOEL
- **Matricule:** 23V2141
- **Email:** makunebijoel@gmail.com

## URL de l'application
https://23V2141.systeme-res30.app

## Description de l'application
Bitwarden est un gestionnaire de mots de passe open-source qui permet aux entreprises et aux particuliers de stocker, partager et gérer leurs identifiants de manière sécurisée. Il offre un chiffrement de bout en bout, un partage d'équipe sécurisé et une intégration avec les principales plateformes.

## Cas d'utilisation en entreprise
- Gestion centralisée des accès aux applications
- Partage sécurisé des identifiants entre équipes
- Conformité aux politiques de sécurité
- Audit des accès aux comptes sensibles

## Instructions pour démarrer l'application
1. Se connecter au VPS: `ssh makune@37.60.250.220`
2. Naviguer vers le dossier: `cd /home/makune/deployments/23V2141`
3. Lancer les conteneurs: `docker compose up -d`
4. Accéder à l'application via: https://23V2141.systeme-res30.app

## Rôle de chaque service dans docker-compose
- **bitwarden:** Contient l'application principale (Vaultwarden)
- **db:** Base de données PostgreSQL pour stocker les données chiffrées

## Variables d'environnement utilisées
- `DB_*`: Configuration de la base de données PostgreSQL
- `SMTP_*`: Configuration pour l'envoi d'emails (récupération de mot de passe, notifications)
- `ADMIN_TOKEN`: Token pour accéder à l'interface d'administration
- `DOMAIN`: Domaine de l'application pour générer les URLs correctes

## Génération du certificat TLS
Le certificat Let's Encrypt wildcard a déjà été configuré sur le serveur. Il se trouve dans `/etc/letsencrypt/live/systeme-res30.app/`

## Persistance des données
Les données sont persistantes grâce aux volumes bind-mount :
- `./bitwarden.app:/data` : Stocke la configuration et les données de Bitwarden
- `./bitwarden.app/postgres:/var/lib/postgresql/data` : Stocke la base de données PostgreSQL

## Comment gagner de l'argent avec cette application
1. **Hébergement SaaS:** Proposer Bitwarden comme service géré pour PME
2. **Support technique:** Offrir des services de configuration et maintenance
3. **Formation:** Former les entreprises à la gestion sécurisée des mots de passe
4. **Intégration:** Développer des connecteurs avec d'autres applications métier
5. **Hébergement privé:** Installer et maintenir Bitwarden sur les serveurs clients

## Dépannage
- Vérifier les logs: `docker compose logs -f`
- Tester Nginx: `sudo nginx -t`
- Redémarrer: `docker compose restart`
