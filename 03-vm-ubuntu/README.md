# Lab 03 – Installer une machine virtuelle Ubuntu et réinitialiser un mot de passe perdu

**Date :** 2 novembre 2026
**Compétences :** virtualisation, installation de Linux, diagnostic d'un problème de connexion, mode de récupération Linux, instantanés (snapshots)

## Contexte

Première machine virtuelle de mon labo maison : une VM Ubuntu, pour m'entraîner à Linux sans risque pour mon PC. L'ISO utilisée est celle vérifiée par empreinte SHA-256 dans le [lab 01](../01-verification-iso/).

Pendant ce lab, j'ai rencontré deux situations courantes en support : un message d'erreur au démarrage, et un mot de passe refusé alors qu'il était correct.

## Environnement

| Élément | Détail |
|---|---|
| Poste hôte | Windows, 16 Go de RAM |
| Disque | J: mécanique (HDD), 1 To libre, réservé aux machines virtuelles |
| Hyperviseur | VirtualBox (type 2) |
| Système invité | Ubuntu Desktop 26.04.1 |
| Fichier d'installation | `J:\ISO\ubuntu-26.04.1-desktop-amd64.iso` (vérifiée, lab 01) |
| Dossier des machines | `J:\VM` |

## Réglages retenus

| Élément | Valeur | Pourquoi |
|---|---|---|
| Installation automatique | Désactivée | Faire l'installation à la main pour comprendre chaque étape |
| Mémoire | 4 Go | Confortable pour Ubuntu, tout en laissant de la marge à l'hôte |
| Processeurs | 2 | Suffisant pour le labo |
| Disque virtuel | 30 Go, taille dynamique | L'espace n'est occupé sur J: qu'au fur et à mesure |
| Réseau | NAT (par défaut) | La VM accède à Internet mais reste cachée derrière le PC |

## Démarche

1. Dans VirtualBox : **Nouvelle**, nom de la VM, choix de l'ISO.
2. Case « Proceed with Unattended Installation » laissée **décochée**. Selon la version de VirtualBox, la question est posée à l'envers (« Skip Unattended Installation ») : il faut lire la phrase exacte avant de cliquer.

   ![Case d'installation automatique laissée décochée](captures/01-installation-automatique-decochee.png)
3. Démarrage sur **« Try or Install Ubuntu »** (mode live), puis langue et clavier en français, réseau par défaut.
4. **Installation interactive**, sélection d'applications par défaut, aucun logiciel propriétaire : dans une VM, le matériel est virtuel et ces pilotes ne servent à rien.
5. Disque : **« Effacer le disque et installer Ubuntu »**. Sans danger ici : Ubuntu ne voit que le disque virtuel de 30 Go, pas les vrais disques du PC.
6. Création du compte (utilisateur `remi`, mot de passe enregistré dans Bitwarden), fuseau horaire Paris, puis installation et redémarrage.
7. Après résolution du problème de connexion (voir plus bas) : arrêt propre de la VM et instantané **« Installation propre - 02/11 »**.

## Problème 1 : message d'erreur DRM au démarrage

**Symptôme :** un message d'erreur commençant par `[drm:...]` défile au démarrage de l'ISO, puis l'installation continue.

**Analyse :** DRM signifie ici *Direct Rendering Manager*, la partie de Linux qui gère l'affichage (rien à voir avec la protection anti-copie). Le message indique que le pilote graphique de la VM n'arrive pas à envoyer un journal à VirtualBox. C'est un message connu et **sans conséquence** avec Ubuntu dans VirtualBox.

**Action :** aucune. Une intervention serait nécessaire seulement si l'écran restait noir et figé (piste : augmenter la mémoire vidéo dans Configuration > Affichage).

## Problème 2 : mot de passe refusé après l'installation

**Symptôme :** à l'écran de connexion, message « Désolé, l'authentification par mot de passe a échoué ». Pourtant, le mot de passe était bien celui noté dans Bitwarden.

**Diagnostic :** si le mot de passe est correct mais refusé, c'est que le système **ne reçoit pas les caractères que l'on croit taper**. Causes vérifiées, de la plus probable à la moins probable :
- copier-coller entre Windows et la VM qui ne fonctionne pas encore (outils invités non installés) ;
- pavé numérique désactivé dans la VM ;
- **disposition de clavier différente** (AZERTY / QWERTY) entre l'installation et la connexion ;
- touches mortes (`^`, `¨`) ou majuscules verrouillées.

**Cause retenue :** disposition de clavier différente entre la saisie du mot de passe à l'installation et la connexion. Le mot de passe était bon, mais les mêmes touches donnaient d'autres caractères.

**Solution : réinitialisation par le mode de récupération.**
1. Redémarrer la VM en maintenant **Maj** (ou **Échap**) pour afficher le menu de démarrage.
2. **Options avancées**, puis la ligne **(recovery mode)**.
3. Dans le menu, choisir **root** pour obtenir une invite de commande en administrateur.
4. Rendre le disque modifiable (il est en lecture seule dans ce mode), puis changer le mot de passe de l'utilisateur :

```bash
mount -o remount,rw /
passwd remi
reboot
```

5. Nouveau mot de passe, simple à taper sur n'importe quel clavier, enregistré immédiatement dans Bitwarden.

## Résultat

- VM Ubuntu installée et accessible.
- Accès rétabli grâce au mode de récupération.
- Instantané « Installation propre - 02/11 » créé.

## Ce que j'ai appris

- **« Je suis sûr de mon mot de passe »** : l'utilisateur a souvent raison sur le mot de passe, mais pas sur ce que le clavier envoie. Vérifier l'indicateur de langue (fr, en…) avant de taper un mot de passe. Sous Windows, le raccourci **Alt+Maj** change la langue du clavier sans qu'on s'en rende compte.
- **Un message qui défile trop vite ne s'ignore pas** : on le retrouve et on vérifie s'il est connu avant de conclure.
- **L'écran « Effacer le disque » est le plus dangereux d'une installation sur un vrai PC** : toujours vérifier quel disque est sélectionné (nom et taille).
- **Angle sécurité :** quiconque a un accès physique à une machine Linux peut réinitialiser un mot de passe par le mode de récupération. C'est pourquoi on **chiffre les disques** des postes d'entreprise (LUKS sous Linux, BitLocker sous Windows).
- **Un instantané n'est pas une sauvegarde** : il dépend du disque virtuel d'origine.

## Compétences mobilisées

`Virtualisation` · `Linux Ubuntu` · `Diagnostic méthodique` · `Réinitialisation de mot de passe` · `Mode de récupération` · `Documentation`
