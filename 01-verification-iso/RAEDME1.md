Lab 01 – Vérifier l'intégrité d'une image ISO (SHA-256)
Date : 2 novembre 2026
Durée : 20 min
Compétences : intégrité des fichiers, hachage SHA-256, PowerShell, bonnes pratiques de téléchargement
Contexte
Avant d'installer un système, un technicien doit s'assurer que l'image téléchargée est identique à celle publiée par l'éditeur : ni corrompue pendant le téléchargement, ni modifiée par un tiers. C'est la première étape de mon labo maison, avant de créer une machine virtuelle Ubuntu sous VirtualBox.
Environnement
Élément	Détail
Poste hôte	Windows, 16 Go de RAM
Hyperviseur	VirtualBox (type 2)
Fichier vérifié	`ubuntu-26.04.1-desktop-amd64.iso`
Source	releases.ubuntu.com (site officiel de Canonical)
Démarche
Téléchargement de l'ISO uniquement depuis le site officiel.
Récupération de la liste des empreintes officielles (fichier `SHA256SUMS`, publié à côté de l'ISO).
Calcul de l'empreinte du fichier téléchargé, sur mon poste, avec PowerShell.
Comparaison des deux empreintes.
Commande utilisée
La commande lit le fichier en entier et en calcule une « empreinte digitale » de 64 caractères. Si un seul octet du fichier change, l'empreinte change complètement.
```powershell
Get-FileHash J:\ISO\ubuntu-26.04.1-desktop-amd64.iso -Algorithm SHA256
```
Résultat
```
Algorithm  Hash
---------  ----
SHA256     601E30FBF5D97759367C632E2C33630665039B7E2158FD068403DA3CCF1BDA1F
```
✅ L'empreinte calculée est identique à celle de la liste officielle : le fichier est intact et peut être utilisé.
Ce que j'ai appris
Le hash prouve l'intégrité, pas l'innocuité : un fichier malveillant intact a lui aussi un hash « correct ». Une empreinte qui correspond signifie seulement que le fichier n'a pas changé.
Le hash prouve l'intégrité, la signature prouve l'origine : la liste des empreintes est hébergée sur le même site que l'ISO. Pour s'assurer qu'elle vient bien de l'éditeur, Ubuntu publie aussi une signature (`SHA256SUMS.gpg`). La vérifier sera mon prochain pas.
Côté SOC, le hash sert aussi d'identifiant : on recherche l'empreinte d'un fichier suspect dans des bases comme VirusTotal pour savoir s'il s'agit d'un malware connu.
Utilité sur le terrain
Support : vérifier une image ou un installeur avant de déployer un poste, pour éviter des pannes difficiles à expliquer.
Sécurité : réflexe de base contre les logiciels piégés et les attaques sur la chaîne d'approvisionnement.
