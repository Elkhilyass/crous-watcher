# 🏠 CROUS Watcher – Alertes Automatiques Logements CROUS

> Un script Python automatisé qui surveille les logements disponibles sur [trouverunlogement.lescrous.fr](https://trouverunlogement.lescrous.fr) et envoie un email dès qu'un **nouveau logement apparaît** dans les zones surveillées.

Le script s'exécute automatiquement **toutes les 5 minutes** via **GitHub Actions** — même si votre PC est éteint.

---

## 🚀 Fonctionnalités

- 🔍 Surveillance de plusieurs zones simultanément (Nanterre, Paris, Versailles, etc.)
- 🆕 Détection uniquement des **nouveaux logements**
- 📧 Envoi automatique d'email à chaque nouvelle annonce
- ⏰ Exécution **24h/24** sans PC allumé
- 💾 Persistance de l'état entre chaque exécution
- 💸 **100% gratuit** via GitHub Actions

---

## 📦 Structure du Projet

```
.
├── crous_watch.py
├── requirements.txt
├── known_accommodations.json
└── .github/
    └── workflows/
        └── run.yml
```

---

## ⚙️ Installation

### 1️⃣ Cloner le projet

```bash
git clone https://github.com/Elkhilyass/crous-watcher.git
cd crous-watcher
```

### 2️⃣ Installer les dépendances

> *(Optionnel si vous utilisez uniquement GitHub Actions)*

```bash
pip install -r requirements.txt
```

---

## 🔐 Configuration Email

Le projet utilise **Gmail SMTP**. Suivez les étapes ci-dessous.

### Étape 1 – Activer la double authentification (2FA)

Dans votre compte Google :
→ **Sécurité** → **Vérification en 2 étapes** → Activer

### Étape 2 – Créer un mot de passe d'application

→ **Sécurité** → **Mots de passe d'application**  
→ Générer un mot de passe pour **"Mail"**  
→ Copier la clé générée (16 caractères)

---

## 🔑 Configuration des GitHub Secrets

Dans votre dépôt GitHub :  
**Settings → Secrets and variables → Actions → New repository secret**

Ajoutez les 3 secrets suivants :

| Nom | Valeur |
|-----|--------|
| `SMTP_USER` | `votre_email@gmail.com` |
| `SMTP_PASSWORD` | `mot_de_passe_d_application` |
| `EMAIL_TO` | `email_qui_reçoit_les_alertes` |

> ⚠️ Ne pas mettre de guillemets ni de `=` dans les noms des secrets.

---

## ⏰ Automatisation avec GitHub Actions

Le fichier `.github/workflows/run.yml` déclenche :

- ✅ Une exécution **toutes les 5 minutes**
- ✅ Ou **manuellement** via *"Run workflow"* dans l'onglet Actions

À chaque exécution, le workflow :

1. Lance une VM Ubuntu
2. Installe Python et les dépendances
3. Exécute le script de surveillance
4. Met à jour `known_accommodations.json`
5. **Commit automatiquement** si un changement est détecté

---

## 🌍 Adapter à une autre ville

Pour surveiller une autre ville, modifiez la liste `URLS` dans `crous_watch.py` :

```python
URLS = [
    "URL_1",
    "URL_2",
]
```

**Comment trouver les bonnes URLs :**

1. Aller sur [trouverunlogement.lescrous.fr](https://trouverunlogement.lescrous.fr)
2. Filtrer par ville souhaitée
3. Copier l'URL générée dans la barre d'adresse
4. La coller dans la liste `URLS`

### 📌 Exemple – Pour Lyon

Vous pouvez surveiller plusieurs communes voisines pour maximiser vos chances :

- Lyon
- Villeurbanne
- Bron
- Vénissieux

> 💡 **Conseil :** surveiller les communes voisines augmente fortement les opportunités disponibles.

---

## 🧠 Comment fonctionne la détection ?

```
1. Le script scrape les logements disponibles sur chaque URL
2. Il extrait : nom, prix, et identifiant unique de chaque logement
3. Il compare avec le fichier known_accommodations.json
4. Si un logement est nouveau → email envoyé immédiatement
5. Sinon → aucune notification
```

---

## 🧪 Lancer manuellement

Dans GitHub :  
**Actions → CROUS Watcher → Run workflow**

- **Première exécution** : au premier lancement, le script enverra un email avec **tous** les logements actuellement disponibles (ainsi tu sais que le code fonctionne.). Les exécutions suivantes ne notifieront que les vraies nouveautés.
---

## 📜 Logs & Debug

Les logs sont visibles dans :  
**Actions → sélectionner un Run → run-script → Run script**

Tous les `print()` du script y apparaissent en temps réel.

---

## 💾 Persistance des données

Le fichier `known_accommodations.json` est automatiquement :

- mis à jour à chaque exécution
- **commit dans le repo** pour conserver l'état entre les runs

Cela évite les doublons et garantit que vous ne recevez des alertes que pour les **vraies nouveautés**.

---

## ⚠️ Limitations

- Dépend de la structure HTML du site CROUS
- Si le site modifie son code HTML, les sélecteurs CSS devront être adaptés dans le script
- **GitHub Actions** : les runs schedulés peuvent avoir un délai de 10-15 minutes en période de forte charge (c'est normal, GitHub ne garantit pas l'exécution exacte à la minute près)

---

## 🎯 Résultat

Vous recevez un email dès qu'un nouveau logement apparaît dans les zones surveillées — sans rien faire de votre côté.

---

## 📄 Licence

Usage **personnel et éducatif** uniquement.

Made with ❤️ by Ilyass :)
