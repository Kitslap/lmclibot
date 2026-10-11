

<video src="https://github.com/user-attachments/assets/084c9ce3-e4ba-46b4-aa1a-32705d4b333a" controls width="100%"></video>


# LMClibot

**Ton agent IA personnel, qui pilote Claude Code pour toi.**

LMClibot est un petit robot qui flotte au-dessus de ton bureau Windows. Tu lui confies une tâche :
il lance Claude Code et ses agents, te montre la réponse dans une bulle, et t'arrête quand une
commande mérite ton avis avant de s'exécuter.

> Ce dépôt ne contient que les **versions compilées** (onglet *Releases*). LMClibot est gratuit,
> mais son code source n'est pas publié.

## Prérequis

- Windows 10 ou 11 (64 bits). WebView2 est installé par l'installeur s'il manque.
- Claude Code installé et connecté à ton compte (voir la documentation d'Anthropic).
  LMClibot n'a aucune intelligence propre et ne demande aucune clé d'API.

## Installer

1. Télécharge `LMClibot-<version>-setup.exe` dans la dernière *Release* (installation sans droits
   administrateur), ou `LMClibot-<version>-portable.exe` pour l'exécuter sans installation.
2. Vérifie l'empreinte : `Get-FileHash .\LMClibot-<version>-setup.exe` doit correspondre à la ligne
   de `SHA256SUMS.txt`.
3. Le binaire n'est pas encore signé : Windows SmartScreen peut afficher « Windows a protégé votre
   ordinateur ». Clique sur *Informations complémentaires › Exécuter quand même* si l'empreinte
   correspond.

## Mises à jour

LMClibot se met à jour tout seul : il vérifie les versions publiées ici au démarrage puis toutes
les 24 h, télécharge la nouvelle en arrière-plan et l'installe quand tu le fermes. La fenêtre
**À propos** (menu du robot ou de l'icône près de l'horloge) affiche ta version, propose
« Mettre à jour maintenant » et permet de couper les mises à jour automatiques.

Chaque version publie `SHA256SUMS.txt` et sa signature `SHA256SUMS.txt.sig` (Ed25519). L'app
refuse toute mise à jour dont la signature ne correspond pas à la clé de publication qu'elle
embarque.

## Sécurité

Chaque version est construite depuis les sources par une intégration continue, puis analysée par
Trivy (dépendances, binaire, secrets) et govulncheck (bibliothèque standard Go). Aucune version
n'est publiée avec une vulnérabilité HIGH ou CRITICAL corrigeable. La nomenclature logicielle
(SBOM CycloneDX) est jointe à chaque version.

LMClibot fonctionne localement et n'envoie aucune donnée à son auteur. Seule la vérification des
mises à jour contacte github.com (désactivable).

## Licence et mentions

Gratuiciel, tous droits réservés — voir `LICENCE.txt` dans chaque version. Composants tiers :
`THIRD-PARTY-NOTICES.txt`.

LMClibot est un outil indépendant, **non affilié à Anthropic** ni approuvé par elle. « Claude » et
« Claude Code » sont des marques d'Anthropic, PBC.
