# Déploiement Bitwarden - INF3611

## Étudiant
- **Nom:** MAKUNE KOUGOUM BIJOEL  
- **Matricule:** 23V2141  
- **Email:** makunebijoel@gmail.com

## URL de l'application
**https://23V2141.systeme-res30.app**

## Description de l'application
Bitwarden (Vaultwarden) est un gestionnaire de mots de passe open-source. Il permet de stocker, organiser et partager des identifiants de connexion de manière sécurisée avec un chiffrement de bout en bout. Les utilisateurs peuvent y enregistrer leurs mots de passe, informations de carte bancaire, notes sécurisées et autres données sensibles.

## Instructions de démarrage
1. Se connecter au VPS via SSH : `ssh makune@37.60.250.220`
2. Naviguer vers le dossier de déploiement : `cd /home/makune/deployments/23V2141`
3. Démarrer les conteneurs Docker : `docker compose up -d`
4. Vérifier que les services tournent : `docker compose ps`
5. Accéder à l'application via : **https://23V2141.systeme-res30.app**

## Rôle de chaque service du docker-compose.yml
- **bitwarden :** Conteneur principal qui exécute l'application Vaultwarden (interface web + API)
- **db :** Conteneur de base de données PostgreSQL qui stocke toutes les données chiffrées de l'application

## Variables d'environnement utilisées dans docker-compose.yml
- `DB_HOST=db` : Nom du service de base de données dans le réseau Docker
- `DB_NAME=bitwarden` : Nom de la base de données PostgreSQL
- `DB_USER=bitwarden` : Utilisateur pour se connecter à la base de données
- `DB_PASSWORD=SecurePassword123!` : Mot de passe de la base de données
- `SMTP_HOST=smtp.gmail.com` : Serveur SMTP pour l'envoi d'emails
- `SMTP_PORT=587` : Port SMTP avec chiffrement STARTTLS
- `SMTP_USER=makunebijoel@gmail.com` : Adresse email d'expédition
- `SMTP_PASSWORD=16Juin2005` : Mot de passe d'application Gmail
- `DOMAIN=23V2141.systeme-res30.app` : Domaine de l'application
- `ROOT_URL=https://23V2141.systeme-res30.app/` : URL complète de l'application
- `ADMIN_TOKEN=SuperSecureAdminToken123!` : Token pour accéder au panneau d'administration

## Rôle éventuel de l'application dans une entreprise
Dans une entreprise, Bitwarden permet :
1. **Gestion centralisée** des identifiants de tous les services utilisés
2. **Partage sécurisé** de mots de passe entre membres d'une équipe
3. **Contrôle d'accès** avec différents niveaux de permission
4. **Audit et traçabilité** des consultations et modifications
5. **Conformité** avec les politiques de sécurité informatique
6. **Prévention** des fuites de données par mots de passe faibles

## Rôle de Let's Encrypt et de Certbot
Let's Encrypt est une autorité de certification qui fournit gratuitement des certificats SSL/TLS pour sécuriser les sites web. Certbot est l'outil client qui automatise la demande, l'installation et le renouvellement de ces certificats. Dans notre cas, un certificat wildcard (*.systeme-res30.app) a été généré pour sécuriser tous les sous-domaines.

## Contenu du fichier de configuration NGINX
Le fichier `/etc/nginx/sites-available/23V2141.conf` contient :
1. Redirection automatique HTTP → HTTPS
2. Configuration SSL avec le certificat wildcard Let's Encrypt
3. Reverse proxy vers le port 6690 (conteneur Bitwarden)
4. Support des WebSockets pour les notifications en temps réel
5. Headers de sécurité (X-Frame-Options, X-Content-Type-Options)
6. Configuration des logs d'accès et d'erreur spécifiques

## Étapes de génération du certificat TLS avec certbot et Let's Encrypt
Le certificat wildcard a été généré avec ces commandes :
```bash
sudo certbot certonly --manual \
  --preferred-challenges=dns \
  -d *.systeme-res30.app \
  -d systeme-res30.app \
  --agree-tos \
  --manual-public-ip-logging-ok
```
Étapes :
1. Lancement de la commande Certbot avec validation DNS
2. Ajout d'un enregistrement TXT DNS spécifique (`_acme-challenge.systeme-res30.app`)
3. Attente de la propagation DNS
4. Validation et génération du certificat

## Emplacement des fichiers du certificat
Les fichiers du certificat se trouvent dans :  
`/etc/letsencrypt/live/systeme-res30.app/`

Fichiers présents :
- `fullchain.pem` : Certificat complet avec chaîne intermédiaire
- `privkey.pem` : Clé privée du certificat
- `cert.pem` : Certificat seul
- `chain.pem` : Chaîne de certificats intermédiaires

---
 
**Cour :** INF3611 - Administration Système - Université de Yaoundé 1  
**Option :** Systèmes et Réseaux - L3
