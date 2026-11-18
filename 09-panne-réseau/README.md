# Lab 09 – Panne réseau simulée, traitée comme un ticket

## Contexte
VM Windows 11 (VirtualBox). Objectif : diagnostiquer une perte de réseau
avec une méthode de support et la documenter comme un ticket helpdesk.

## Ticket
- **Titre** : Pas d'accès Internet – poste Windows11-Labo
- **Priorité** : basse (1 poste, contournement possible)
- **Symptôme** : la page web ne s'affiche pas

![Icône réseau barrée dans la barre des tâches](captures/01-icone-reseau-panne.png)

## Diagnostic
| Étape | Commande | Résultat | Conclusion |
|---|---|---|---|
| 1 | ipconfig | "Média déconnecté" | carte réseau détectée mais pas de liaison physique, donc pas d'accès Internet |
| 2 | ping 8.8.8.8 | "Impossible de joindre l'hôte de destination" | DNS public de Google injoignable |
| 3 | ping google.fr | "Impossible de joindre le site" | pas de réponse |

**Étape 1 – ipconfig pendant la panne**

![ipconfig pendant la panne : Média déconnecté](captures/02-ipconfig-panne.png)

**Étape 2 – ping 8.8.8.8 pendant la panne**

![ping 8.8.8.8 pendant la panne : hôte de destination injoignable](captures/03-ping-ip-panne.png)

## Cause
Carte réseau virtuelle déconnectée dans VirtualBox (simule un câble réseau débranché).

## Résolution et vérification
Simulation du rebranchement du câble : carte réseau reconnectée dans VirtualBox (« Virtual Cable Connected » coché).

![Réglage réseau de la VM dans VirtualBox, câble virtuel reconnecté](captures/04-virtualbox-cable-reconnecte.png)

Vérifications : de nouveau `ipconfig`, `ping 8.8.8.8` et `ping google.fr`. Tous répondent et la page Internet s'affiche.

![Vérification après réparation : adresse IP obtenue, les deux ping répondent](captures/06-verification-ping.png)

## Ce que j'en retiens
- Différence entre « Média déconnecté » et une adresse 169.254.x.x
- Angle sécurité : une panne sur plusieurs postes peut être un incident de sécurité plus grave
