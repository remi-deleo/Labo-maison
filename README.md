# Labo maison – Rémi DE LEO

Comptes rendus de mes travaux pratiques, réalisés dans le cadre de ma reconversion vers le **support informatique**, puis la **cybersécurité** (objectif : analyste SOC).

Chaque lab suit la même structure : contexte, environnement, démarche, commandes, résultat et ce que j'en retiens.

## Environnement

- Poste Windows, 16 Go de RAM
- VirtualBox, avec des machines virtuelles Ubuntu et Windows 11

## Labs

| N° | Lab | Thème | Date |
|---|---|---|---|
| 01 | [Vérifier l'intégrité d'une ISO](01-verification-iso/) | Sécurité · PowerShell | 02/11/2026 |
| 02 | [Sécuriser ses comptes : MFA et gestionnaire de mots de passe](02-securiser-comptes-mfa/) | Sécurité · IAM (Identity and Access Management) | 02/11/2026 |
| 03 | [Installer une VM Ubuntu et réinitialiser un mot de passe perdu](03-vm-ubuntu/) | Virtualisation · Linux · dépannage | 02/11/2026 |
| 04 | [Installer une VM Windows 11 sous VirtualBox](04-vm-windows-11/) | Virtualisation · dépannage d'affichage | 03/11/2026 |
| 05 | [Binaire, encodage et hash : les bases vues par un analyste](05-binaire-encodage-hash/) | Fondamentaux · Base64 · SHA-256 | 09/11/2026 |
| 06 | [La carte d'identité d'un poste : VM et PC hôte](06-carte-identite-poste/) | Support · inventaire · comptes locaux | 10/11/2026 |

## Autres projets

- [diagnostic-pc](https://github.com/remi-deleo/diagnostic-pc) : script PowerShell de diagnostic d'un poste
- [carto-reseau](https://github.com/remi-deleo/carto-reseau) : cartographie du réseau maison avec nmap

## Comment ce dépôt est réalisé

Tous les labs sont faits par moi, sur mes propres machines virtuelles
(VirtualBox, Windows 11 et Ubuntu) : installation, commandes, captures
d'écran et dépannage.

Je me fais aider par un assistant IA (Claude) pour structurer et relire
les comptes rendus, comme on le ferait avec un collègue. Les premiers
commits ont été poussés avec son aide, d'où sa présence dans les
contributeurs. Depuis novembre 2026, je fais tous les commits moi-même.

Chaque commande publiée ici, je sais l'expliquer et la refaire.
