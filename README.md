# 🏠 CROUS Watcher – Alertes Automatiques Logements CROUS

Un script Python automatisé qui surveille les logements disponibles sur le site  
https://trouverunlogement.lescrous.fr

Il envoie un email uniquement lorsqu’un **nouveau logement apparaît** dans les zones surveillées.

Le script est exécuté automatiquement toutes les 10 minutes via **GitHub Actions**.

---

# 🚀 Fonctionnalités

- Surveillance de plusieurs zones (Nanterre, Paris, Versailles, etc.)
- Détection uniquement des **nouveaux logements**
- Envoi automatique d’email
- Exécution 24h/24 même si votre PC est éteint
- Persistance de l’état entre chaque exécution
- 100% gratuit (GitHub Actions)

---

# 📦 Structure du Projet

.
├── crous_watch.py
├── requirements.txt
├── known_accommodations.json
└── .github/
└── workflows/
└── run.yml


---

# ⚙️ Installation

## 1️⃣ Cloner le projet

git clone https://github.com/Elkhilyass/crous-watcher.git
cd crous-watcher
##2️⃣ Installer les dépendances (optionnel si usage GitHub Actions uniquement)
pip install -r requirements.txt
🔐 Configuration Email (IMPORTANT)
Le projet utilise Gmail SMTP.

Étape 1 – Activer la double authentification (2FA)
Dans votre compte Google :

Activer la vérification en 2 étapes

Étape 2 – Créer un mot de passe d’application
Aller dans Sécurité → Mots de passe d’application

Générer un mot de passe pour "Mail"

Copier la clé générée

🔑 Configuration GitHub Secrets
Dans votre dépôt GitHub :

Settings → Secrets and variables → Actions → New repository secret

Ajouter les 3 secrets suivants :

Name	Value
SMTP_USER	votre_email@gmail.com
SMTP_PASSWORD	mot_de_passe_d_application
EMAIL_TO	email_qui_reçoit_les_alertes
⚠️ Ne pas mettre de guillemets ni de "=" dans les noms.

⏰ Automatisation avec GitHub Actions
Le fichier :

.github/workflows/run.yml
déclenche :

une exécution toutes les 5 minutes

ou manuellement via "Run workflow"

Le workflow :

Lance une VM Ubuntu

Installe Python

Installe les dépendances

Exécute le script

Met à jour known_accommodations.json

Commit automatiquement si changement

🌍 Adapter à une autre ville

Si vous recherchez un logement dans une autre ville, il suffit de modifier les URLs dans crous_watch.py.

Dans le fichier :

URLS = [
    "URL_1",
    "URL_2",
]


Remplacez les liens actuels par les URLs correspondant à la ville (ou aux villes proches) que vous souhaitez surveiller.

📌 Exemple – Pour Lyon :

Vous pouvez surveiller :

Lyon

Villeurbanne

Bron

Vénissieux

Aller sur https://trouverunlogement.lescrous.fr

Filtrer par ville

Copier l’URL générée

La coller dans la liste URLS

Vous pouvez ajouter plusieurs villes pour maximiser vos chances.

⚠️ Conseil : surveiller aussi les communes voisines augmente fortement les opportunités disponibles.

🧠 Comment fonctionne la détection ?
Le script extrait :

nom du logement

prix (si disponible)

identifiant unique

Compare avec known_accommodations.json

Si nouveau logement détecté → email envoyé

Sinon → aucune notification

🧪 Lancer manuellement
Dans GitHub :

Actions → CROUS Watcher → Run workflow

📜 Logs & Debug
Les print() sont visibles dans :

Actions → Run → run-script → Run script

📌 Persistance des données
Le fichier :

known_accommodations.json
est automatiquement mis à jour et commit dans le repo
pour conserver l’état entre chaque exécution.

⚠️ Limitations
Dépend de la structure HTML du site CROUS

Si le site change son code HTML, les sélecteurs devront être adaptés

💡 Améliorations possibles
Notification Telegram

Filtrage par prix max

Ajout de Slack/Discord

Stockage sur base de données

Dockerisation complète

🎯 Résultat
Vous recevez un email dès qu’un nouveau logement apparaît
dans les zones surveillées.

📄 Licence
Usage personnel et éducatif.
