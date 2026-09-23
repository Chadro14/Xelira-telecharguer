Xelira Download

«A lightweight, extensible TikTok media downloader interface with a frontend-based licensing layer.»

Xelira Download est une interface web moderne permettant de soumettre une URL TikTok et de récupérer les médias disponibles via une API tierce.

Le projet a été conçu avec une approche frontend-first, sans backend obligatoire, tout en intégrant un système simple de licences temporaires destiné aux démonstrations, projets personnels et distributions contrôlées.

«[!WARNING]
Le système de licence actuel fonctionne entièrement côté client. Il ne doit pas être considéré comme un mécanisme de sécurité ou de protection commerciale. Pour une validation fiable, les licences doivent être vérifiées côté serveur.»

---

✨ Fonctionnalités

- 🎬 Récupération des médias à partir d'une URL TikTok
- ⚡ Interface rapide et responsive
- 📱 Compatible mobile, tablette et desktop
- 🎨 UI moderne avec SVG et CSS natif
- 🔐 Système d'activation par licence
- 👤 Licence associée à un utilisateur
- 📅 Date d'expiration configurable
- ⏳ Calcul automatique des jours restants
- 🚫 Blocage automatique des licences expirées
- 🔄 Renouvellement possible avec le même identifiant de licence
- 💬 Contact WhatsApp pour obtenir ou renouveler une licence
- 🧩 Architecture sans framework
- 📦 Aucun build obligatoire
- 🌐 Déploiement possible sur GitHub Pages, Netlify, Vercel ou hébergement statique

---

🖥️ Aperçu

Le parcours utilisateur est volontairement simple :

                    ┌─────────────────────┐
                    │   Ouverture du site │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Licence enregistrée │
                    │    et valide ?      │
                    └──────┬────────┬─────┘
                           │ Oui     │ Non
                           ▼         ▼
                     ┌─────────┐  ┌──────────────┐
                     │Download │  │ Activation   │
                     │  UI     │  │    écran     │
                     └────┬────┘  └──────┬───────┘
                          │               │
                          │               ▼
                          │        ┌──────────────┐
                          │        │ Licence OK ? │
                          │        └──────┬───────┘
                          │               │
                          └───────────────┘

---

🧱 Stack technique

Le projet utilise volontairement un stack minimaliste.

Technologie| Utilisation
HTML5| Structure
CSS3| Interface et responsive design
JavaScript| Logique applicative
SVG| Icônes et éléments graphiques
LocalStorage| Persistance locale de la licence
TikWM API| Récupération des informations média

Aucun framework JavaScript n'est requis.

Aucune installation de dépendances n'est nécessaire pour la version standalone.

---

📁 Structure

xelira-download/
│
├── index.html
├── README.md
├── LICENSE
└── assets/
    └── ...

Dans la version standalone actuelle, l'ensemble de l'application peut également être contenu dans un seul fichier HTML.

---

🔐 Système de licence

Xelira utilise actuellement un système de licence client-side.

Une licence possède notamment :

const LICENSES = {
  "XELIRA-THIERRY-2026": {
    name: "Thierry",
    expires: "2026-10-23"
  }
};

Propriétés

"name"

Nom associé à la licence.

name: "Thierry"

"expires"

Date d'expiration au format :

YYYY-MM-DD

Exemple :

expires: "2026-10-23"

---

♻️ Renouvellement

Le système permet de conserver le même identifiant de licence.

Exemple :

Licence
XELIRA-THIERRY-2026

Expiration initiale
23/10/2026

Renouvellement

Expiration
23/11/2026

Il suffit de modifier la date côté configuration :

"XELIRA-THIERRY-2026": {
  name: "Thierry",
  expires: "2026-11-23"
}

L'utilisateur peut alors réutiliser son ancienne licence après renouvellement.

---

💾 Persistance

La licence activée est enregistrée dans le navigateur avec :

localStorage

La clé utilisée est :

xelira_license

Cela permet de conserver l'activation après un rechargement de la page.

---

⚠️ Sécurité des licences

Cette section est importante pour les développeurs qui souhaitent utiliser Xelira dans un contexte commercial.

Le système actuel est volontairement frontend-only.

Cela signifie que :

Browser
   │
   ├── JavaScript
   ├── LICENSES
   └── localStorage

Tout ce qui est exécuté dans le navigateur peut potentiellement être inspecté ou modifié par l'utilisateur.

Un utilisateur techniquement expérimenté peut donc :

- modifier le JavaScript ;
- modifier les dates ;
- supprimer/modifier le "localStorage" ;
- contourner la validation ;
- extraire les licences présentes dans le bundle.

Conclusion

Ce système convient pour :

- prototypes ;
- démonstrations ;
- projets personnels ;
- distributions simples ;
- expériences frontend.

Il ne convient pas comme système de DRM ou de licence commerciale sécurisé.

---

🚀 Architecture recommandée pour la production

Pour une version commerciale, il est recommandé de déplacer la logique de licence vers une API.

Architecture recommandée :

                    ┌───────────────────┐
                    │     Frontend      │
                    │  Xelira Download  │
                    └─────────┬─────────┘
                              │
                         HTTPS / API
                              │
                              ▼
                    ┌───────────────────┐
                    │     Backend       │
                    │ License Service   │
                    └─────────┬─────────┘
                              │
                              ▼
                    ┌───────────────────┐
                    │     Database      │
                    │                   │
                    │ license           │
                    │ user              │
                    │ expiration        │
                    │ status            │
                    └───────────────────┘

Le frontend ne devrait jamais contenir la liste complète des licences.

Une API pourrait exposer par exemple :

POST /api/licenses/activate

{
  "license": "XELIRA-THIERRY-2026"
}

Réponse :

{
  "valid": true,
  "user": "Thierry",
  "expiresAt": "2026-11-23T00:00:00Z"
}

Le serveur reste alors la source de vérité.

---

🔌 API TikWM

Le projet utilise actuellement :

https://tikwm.com/api/?url=

Exemple :

const response = await fetch(
  "https://tikwm.com/api/?url=" + encodeURIComponent(url)
);

Les résultats peuvent notamment fournir différents médias selon ce que l'API retourne :

play
hdplay
wmplay
music

Le projet ne garantit pas la disponibilité permanente de ces champs.

L'API étant un service tiers, son comportement, ses limites et sa disponibilité peuvent évoluer.

---

🛡️ Gestion des erreurs

Le frontend vérifie notamment :

- URL vide ;
- URL TikTok invalide ;
- erreur HTTP ;
- réponse API invalide ;
- absence de média ;
- licence inexistante ;
- licence expirée.

Exemple :

if (!data || data.code !== 0 || !data.data) {
  throw new Error("Aucune vidéo trouvée.");
}

---

📱 Responsive Design

L'interface est conçue pour fonctionner sur :

- smartphones ;
- tablettes ;
- ordinateurs portables ;
- écrans desktop.

Aucune librairie CSS externe n'est nécessaire.

---

⚙️ Installation

Option 1 — utilisation directe

Clone le repository :

git clone https://github.com/USERNAME/xelira-download.git

Entre dans le dossier :

cd xelira-download

Puis ouvre :

index.html

dans ton navigateur.

---

Option 2 — serveur local

Avec Python :

python -m http.server 8080

Puis ouvre :

http://localhost:8080

---

🌐 Déploiement

Le projet étant statique, il peut être déployé sur de nombreux services.

GitHub Pages

Repository
    ↓
Settings
    ↓
Pages
    ↓
Deploy from branch

Vercel

Importe simplement le repository GitHub.

Netlify

Connecte le repository GitHub puis déploie le projet.

Aucun build command n'est nécessaire pour la version standalone.

---

🧪 Développement

Le projet ne nécessite actuellement aucune installation :

npm install

n'est pas nécessaire.

Le développement peut être effectué directement avec :

HTML
CSS
JavaScript

Pour un environnement plus avancé, une migration vers une architecture modulaire peut être envisagée.

---

🛣️ Roadmap

Frontend

- [x] Interface responsive
- [x] SVG
- [x] Validation d'URL
- [x] Téléchargement média
- [x] Système de licence
- [x] Expiration
- [x] Renouvellement
- [x] Interface WhatsApp
- [ ] Historique local
- [ ] Dark/light mode
- [ ] Internationalisation

Backend

- [ ] API de licences
- [ ] Base de données
- [ ] Création de licences
- [ ] Révocation de licences
- [ ] Renouvellement automatique
- [ ] Limitation du nombre d'appareils
- [ ] Dashboard administrateur
- [ ] Authentification
- [ ] Logs d'utilisation

Paiement

Une future version pourrait intégrer un système de paiement automatisé.

Le flux pourrait devenir :

Utilisateur
     │
     ▼
Paiement
     │
     ▼
Confirmation
     │
     ▼
Création automatique de licence
     │
     ▼
Activation

---

🔒 Bonnes pratiques pour une version production

Si tu transformes Xelira en SaaS commercial :

- ne stocke pas les secrets dans le frontend ;
- ne considère jamais "localStorage" comme une source de confiance ;
- valide les licences côté serveur ;
- utilise HTTPS ;
- protège les endpoints avec rate limiting ;
- journalise les activations importantes ;
- utilise des identifiants de licence suffisamment aléatoires ;
- évite d'exposer les informations sensibles dans le bundle JavaScript ;
- vérifie les conditions d'utilisation des services tiers utilisés.

---

⚖️ Utilisation responsable

Xelira Download est une interface technique.

L'utilisateur est responsable de l'utilisation qu'il fait des médias récupérés.

Vous devez respecter :

- les droits d'auteur ;
- les droits des créateurs ;
- les conditions d'utilisation des plateformes concernées ;
- les lois applicables dans votre juridiction.

Xelira ne revendique aucun droit sur les contenus accessibles via les services tiers.

---

🤝 Contribution

Les contributions sont les bienvenues.

Pour proposer une modification :

git fork

Crée une branche :

git checkout -b feature/ma-feature

Effectue tes modifications puis :

git add .
git commit -m "feat: add my feature"
git push origin feature/ma-feature

Ensuite, ouvre une Pull Request.

Convention de commits recommandée

feat: nouvelle fonctionnalité
fix: correction
refactor: refactorisation
docs: documentation
style: interface
perf: performance
security: sécurité
chore: maintenance

---

📄 Licence

Ajoute ici la licence que tu souhaites utiliser pour le repository.

Exemple :

MIT License

Si tu souhaites conserver davantage de contrôle sur la redistribution commerciale, choisis une licence adaptée à ton objectif avant de publier le repository.

---

👨‍💻 Auteur

Xelira Studio

Projet développé pour expérimenter une interface de téléchargement moderne, une architecture frontend-first et un système de licences côté client.

Open Source

Retrouve les ressources et les actualités du projet :

https://whatsapp.com/channel/0029Vb6aMu46RGJOcGGe1H1O

---

⭐ Soutenir le projet

Si Xelira Download t'est utile :

- ⭐ Star le repository
- 🐛 Signale les bugs
- 💡 Propose des améliorations
- 🔀 Contribue au projet
- 📢 Partage le projet avec d'autres développeurs

---

«Xelira Download — Build. Customize. Ship.»