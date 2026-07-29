# Échecs — Guide d'installation

Ce guide explique comment installer et configurer le jeu d'Échecs dans Bot Designer For Discord (BDFD) avec Components V2.

## Prérequis

- Un bot créé dans BDFD.
- Components V2 activé pour ton bot.
- La variable `chess_state` créée (valeur par défaut : vide).

## Variables à créer

Dans le tableau de bord BDFD, va dans **Variables** et crée :

| Nom | Valeur |
|-----|--------|
| `chess_state` | (vide) |

Cette variable stocke l'état complet de chaque partie : `fen|whiteID|blackID|gameID|status|lastMove|lastMoveSAN|theme`.

## API externe

Ce jeu nécessite une API HTTP sans état pour valider les coups et rendre l'échiquier. L'API utilisée est `http://chess.nexusify.co`.

## Installation

### 1. Créer la variable

Crée `chess_state` avec une valeur vide.

### 2. Créer les trois commandes

Tu dois créer 3 commandes dans BDFD :

1. **Commande principale** — voir [`Commande_Echecs.md`](./Commande_Echecs.md)
2. **Callback 1** — voir [`Callback 1 ($onInteraction).md`](./Callback%201%20%28$onInteraction%29.md)
3. **Callback 2** — voir [`Callback 2 ($onInteraction).md`](./Callback%202%20%28$onInteraction%29.md)

Pour chaque commande, copie le déclencheur et le code depuis le fichier correspondant.

### 3. Jouer !

Tape `!chess @adversaire` dans n'importe quel salon où ton bot peut lire les messages.

## Remarques

- L'état de la partie est stocké par utilisateur (challenger), donc plusieurs joueurs peuvent jouer simultanément.
- Chaque partie a un ID unique. Si tu en commences une nouvelle, les boutons de l'ancienne cessent de fonctionner.
- Le challenger peut choisir le thème de l'échiquier (Green, Blue, Brown, Purple) avant que l'adversaire accepte.
- Utilise 🏳️ Abandonner pour abandonner, ou 🤝 Proposer nul pour proposer un nul.

## Commande Slash (optionnel)

Si tu veux utiliser `/chess` en plus de `!chess`, configure la commande slash dans BDFD :

| Champ | Valeur |
|-------|--------|
| Option name | `adversaire` |
| Option type | User |
| Option Required | Oui |

Une fois configurée, les utilisateurs pourront utiliser `/chess adversaire:@adversaire`.
