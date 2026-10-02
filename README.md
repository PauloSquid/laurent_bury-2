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

Le déploiement du site :

```bash
npm run build
npx firebase-tools deploy --only hosting
```

Les redirections sont déjà prévues dans `firebase.json` pour le routeur Angular.
