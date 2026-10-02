# Laurent Bury, traducteur

Refonte du site [laurentburytraducteur.com](https://www.laurentburytraducteur.com) : même contenu, mise en page éditoriale, catalogue des traductions et administration.

## Lancer le site

```bash
npm start
```

Le site s’ouvre sur http://localhost:4200. Le catalogue (278 ouvrages) est chargé depuis `public/data/livres.json`.

L’administration est sur http://localhost:4200/admin. Tant que Firebase n’est pas renseigné, le mot de passe est celui de `localAdminPassword` dans `src/environments/environment.ts` (`atelier` par défaut). Les ajouts et modifications restent alors dans ce navigateur.

## Brancher Firebase

Firebase suffit : lecture publique du catalogue, écriture réservée au compte administrateur. NestJS n’est pas nécessaire.

1. Créer un projet Firebase, activer Authentication (e-mail / mot de passe) et créer un utilisateur.
2. Créer une base Firestore.
3. Coller la configuration Web dans `src/environments/environment.ts`.
4. Déployer les règles : `npx firebase-tools deploy --only firestore:rules`
5. Se connecter à `/admin` avec l’e-mail Firebase, puis cliquer sur « Importer les ouvrages ».

Le déploiement du site se fait tout seul à chaque push sur `main`, via `.github/workflows/deploy.yml`. Il publie l’hébergement et les règles Firestore sur le projet `laurentburytraducteur`.

Une seule chose se configure à la main, avec le compte propriétaire du projet Firebase :

1. [Comptes de service Google Cloud](https://console.cloud.google.com/iam-admin/serviceaccounts?project=laurentburytraducteur) → **Créer un compte de service**, par exemple `github-deploy`.
2. Lui donner les rôles **Administrateur Firebase Hosting**, **Administrateur Firebase Rules** et **Consommateur Service Usage**.
3. Onglet **Clés** → **Ajouter une clé** → **JSON**. Ce fichier ne se commit pas.
4. Sur GitHub, dépôt **PauloSquid/laurent_bury-2** → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**. Nom : `FIREBASE_SERVICE_ACCOUNT`. Valeur : le contenu entier du fichier JSON.
5. Pousser sur `main`. L’onglet **Actions** construit le site puis le publie sur `https://laurentburytraducteur.web.app` et `https://laurentburytraducteur.firebaseapp.com`.

Les redirections sont déjà prévues dans `firebase.json` pour le routeur Angular.
