# Cousinade Party — démarrage gratuit

Mini-application familiale web : quiz corrigés côté base de données, classement partagé, défis soumis par les joueurs et validés par un administrateur.

## Contenu
- `index.html` : interface mobile et logique cliente.
- `schema.sql` : tables, politiques de sécurité et fonctions Supabase.
- `README.md` : guide d'installation.

## 1. Créer le backend gratuit
1. Crée un projet sur https://supabase.com/ et garde-le en région proche de la majorité des joueurs.
2. Dans le tableau de bord Supabase, ouvre **Authentication → Sign In / Providers** (le nom du menu peut évoluer) et active **Anonymous Sign-Ins**.
3. Ouvre **SQL Editor**, colle le contenu complet de `schema.sql`, puis exécute-le.
4. Dans **Project Settings → API** (ou Connect), copie l'URL du projet et la clé publique `anon` / publishable. Ne copie JAMAIS la clé `service_role` / secret dans le navigateur.
5. Dans `index.html`, remplace :
   - `COLLER_ICI_URL_SUPABASE` par l'URL Supabase ;
   - `COLLER_ICI_CLE_ANON_SUPABASE` par la clé publique ;
   - `COUSINS2026` par un code familial choisi. Mets exactement le même code dans la ligne `insert into public.families...` de `schema.sql` avant de l'exécuter, ou modifie la ligne de la famille directement depuis l'éditeur SQL.
6. Important : dans le fichier HTML, le code familial est visible dans le navigateur. Il sert de filtre d'entrée convivial, pas de secret fort. Ne l'utilise pas pour protéger des données sensibles.

## 2. Créer le compte administrateur
1. Ouvre la page de l'application et rejoins la famille avec un pseudo que tu veux garder comme administrateur.
2. L'identifiant technique du joueur s'affiche dans le panneau admin, mais ce panneau n'est pas encore visible tant que tu n'as pas accordé le rôle.
3. Dans Supabase → **Table Editor → profiles**, repère la ligne correspondant à ton pseudo.
4. Modifie `role` de `player` à `admin`.
5. Recharge la page. Le bouton **Admin** apparaît : tu peux créer quiz et défis et valider les participations.

**Sécurité importante :** l'authentification anonyme signifie que la session est liée au navigateur/appareil, pas à une identité vérifiée. Si un joueur efface ses données de navigateur, il peut perdre son compte. Le code familial seul ne prouve pas l'appartenance à la famille. Pour une utilisation durable, passe ensuite à une connexion e-mail/mot de passe ou un lien magique.

## 3. Mettre en ligne gratuitement avec GitHub Pages
1. Crée un compte GitHub si nécessaire.
2. Crée un dépôt public, par exemple `cousinade-party`.
3. Ajoute `index.html` à la racine du dépôt. Ne publie jamais de clé `service_role`.
4. Dans le dépôt, ouvre **Settings → Pages**, choisis le déploiement depuis la branche `main` et le dossier `/ (root)`, puis enregistre.
5. GitHub affiche l'URL de ton site après le déploiement. Ouvre-la sur téléphone et ordinateur, puis partage le lien dans le groupe Facebook.
6. Si tu changes le code de l'application, modifie `index.html` et pousse les changements sur GitHub.

## 4. Comment jouer
- Les joueurs saisissent le code familial et leur pseudo.
- Chaque quiz est jouable une seule fois par compte ; une bonne réponse attribue automatiquement les XP définis par l'administrateur.
- Pour un défi créatif, le joueur soumet une note/description. L'administrateur accepte ou refuse dans l'onglet Admin.
- Le classement additionne les scores de quiz et les points des défis validés.

## 5. Limites et conseils
- La clé `anon` / publishable est prévue pour être publique ; la sécurité dépend des politiques RLS et des fonctions SQL.
- N'expose jamais `service_role` / une clé secrète dans le HTML.
- Les joueurs anonymes peuvent créer plusieurs sessions/comptes et choisir un pseudo libre. Ce MVP ne garantit pas une identité unique par personne.
- Le code familial est présent dans le JavaScript client. Il n'est pas un mécanisme de sécurité robuste.
- Les noms et scores sont visibles aux membres connectés de la famille. Ne collecte pas de données personnelles inutiles.
- Le code de départ utilise un quiz à une seule question par jeu. Une prochaine version pourrait gérer plusieurs questions par quiz, un historique des saisons, des images et une validation anti-triche renforcée.
- Les offres gratuites des fournisseurs ont des quotas, règles d'inactivité et limites qui peuvent évoluer. Vérifie les conditions actuelles de Supabase et GitHub Pages.
- Les règles Meta des groupes sont indépendantes : l'application ne lit ni ne publie automatiquement dans le groupe Facebook. Les joueurs partagent manuellement les liens et les défis.

## Dépannage
- « Code familial incorrect » : le code dans `index.html` doit correspondre à `families.access_code` dans Supabase.
- Bouton Admin absent : vérifie `profiles.role = admin`, puis recharge la page.
- Aucun classement : vérifie que le schéma SQL a été exécuté sans erreur et que le joueur a bien rejoint la famille.
- Impossible de se connecter : vérifie l'activation des connexions anonymes et l'URL/la clé publique Supabase.
