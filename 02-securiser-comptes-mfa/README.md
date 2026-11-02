# Lab 02 – Sécuriser ses comptes : gestionnaire de mots de passe et MFA

**Date :** 1er et 2 novembre 2026
**Durée :** environ 3 h
**Environnement :** PC Windows (Firefox, Chrome), smartphone Android
**Outils :** Bitwarden (gestionnaire de mots de passe), Ente Auth (application d'authentification TOTP)

---

## Contexte

Mes mots de passe étaient enregistrés dans le gestionnaire de Google, et l'accès à mon compte GitHub était protégé par une double authentification par SMS. Cette configuration présente deux faiblesses :

- **Un point unique de défaillance** : un compte Google compromis donne accès à tous les autres comptes.
- **Le SMS est le facteur MFA (authentification multifacteur) le plus faible** : il est exposé au SIM swapping (vol de la ligne auprès de l'opérateur) et à l'interception.

C'est le type de situation qu'un technicien support rencontre chez un utilisateur ou dans une petite structure sans politique de sécurité des accès.

## Objectifs

1. Centraliser les mots de passe dans un gestionnaire dédié, indépendant de Google.
2. Remplacer la MFA par SMS par une application d'authentification (TOTP).
3. Protéger les comptes critiques (gestionnaire de mots de passe, Google, GitHub) par une MFA.
4. Prévoir la récupération d'accès en cas de perte du téléphone.

## Démarche

### 1. Gestionnaire de mots de passe
- Installation de **Bitwarden** (open source, audité, gratuit) : extension Firefox sur PC et application Android.
- **Import** des mots de passe exportés depuis le gestionnaire Google.
- **Désactivation** de l'enregistrement des mots de passe dans Firefox et dans Chrome, pour qu'un seul outil fasse foi.
- Sur Android : Bitwarden défini comme **service de saisie automatique**, avec déverrouillage biométrique et verrouillage automatique du coffre.

### 2. Application d'authentification
- Installation d'**Ente Auth** sur Android (open source, sauvegarde chiffrée de bout en bout).
- Activation de la **double authentification sur Bitwarden** via Ente Auth.

### 3. Migration des comptes critiques
| Compte | Avant | Après |
|---|---|---|
| Bitwarden | — | TOTP (Ente Auth) |
| GitHub | SMS | TOTP (Ente Auth), SMS supprimé |
| Google | — | TOTP (Ente Auth) + clé d'accès (passkey) du téléphone |

Chaque changement a été **testé** (déconnexion puis reconnexion) avant de supprimer l'ancienne méthode.

### 4. Plan de récupération
- Codes de secours Google, code de récupération Bitwarden et clé de récupération Ente notés **sur papier**, rangés hors ligne.
- Choix volontaire de ne pas stocker ces codes *uniquement* dans Bitwarden, pour éviter une **dépendance circulaire** (les codes qui servent à récupérer Bitwarden seraient enfermés dans Bitwarden).

## Problème rencontré et diagnostic

**Symptôme :** dans l'application Fnac sur Android, c'est le gestionnaire Google qui proposait les identifiants, alors que Bitwarden était déjà défini comme service de saisie automatique d'Android.

**Démarche de diagnostic :**
1. Vérification du service de saisie automatique d'Android : Bitwarden bien sélectionné, et Google désactivé dans les services supplémentaires.
2. Désactivation de la saisie automatique Google à la source (Paramètres > Google > Saisie automatique), puis redémarrage : toujours Google.
3. Observation du comportement : l'application **ouvre en réalité une page Chrome** (onglet personnalisé) pour la connexion. Le réglage d'Android ne s'applique donc pas ; c'est celui de Chrome qui compte.

**Cause :** Chrome utilise son propre gestionnaire de mots de passe, indépendamment du service de saisie automatique d'Android.

**Résolution :** dans Chrome, saisie automatique confiée à un autre service (Bitwarden) et désactivation de l'enregistrement et de la connexion automatique de Google.

**Leçon :** quand un réglage « ne fonctionne pas », vérifier d'abord **quel composant traite réellement la demande**. Ici, l'application elle-même ne gérait pas la connexion, Chrome s'en chargeait.

## Résultat

- Un seul gestionnaire de mots de passe sur PC et smartphone.
- Plus aucune MFA par SMS sur les comptes critiques.
- Accès récupérable en cas de perte ou de vol du téléphone.
- Étape restante : suppression des mots de passe encore stockés chez Google après une semaine d'utilisation, une fois vérifié que rien ne manque dans Bitwarden.

## Ce que j'ai appris

- **Hiérarchie des facteurs MFA** : SMS < application TOTP < clé d'accès (passkey) ou clé physique. La passkey résiste au phishing parce qu'elle est liée au nom de domaine du site.
- **Toujours prévoir la récupération** avant de durcir un accès : une MFA sans plan de secours, c'est un futur ticket « compte bloqué ».
- **Tester avant de supprimer** l'ancienne méthode d'authentification.
- En support, une demande de réinitialisation de MFA (« j'ai perdu mon téléphone ») doit commencer par une **vérification d'identité** : c'est un scénario classique d'ingénierie sociale.

## Compétences mobilisées

`Gestion des identités et des accès` · `MFA / TOTP` · `Passkeys` · `Android` · `Diagnostic méthodique` · `Documentation`
