[English](README.md) · **Français**

# LMClibot

<video src='https://github.com/user-attachments/assets/709a0ffd-c209-4ac4-86d1-915bbe76ca53' width=100%/>></video>

**Ton agent IA de bureau, qui pilote Claude Code ou GitHub Copilot CLI pour toi.**

LMClibot est un petit robot qui flotte au-dessus de ton bureau Windows. Tu lui confies une tâche dans
sa bulle : il la fait exécuter par l'agent en ligne de commande que tu utilises déjà (Claude Code ou
GitHub Copilot CLI), te montre la réponse au fil de l'eau, et t'arrête quand une commande mérite ton
avis avant de s'exécuter. Toute la puissance de l'agent, sans le terminal.

> Ce dépôt ne contient que les **versions compilées** (onglet *Releases*). LMClibot est gratuit,
> mais son code source n'est pas publié.

## Ce que LMClibot permet de faire

- **Parler à ton agent depuis une bulle.** Réponse en direct, outils utilisés visibles, captures
  d'écran et fichiers à joindre, file de messages pendant que l'agent travaille, bouton Stop.
- **Garder plusieurs sessions.** La fenêtre Sessions liste tes conversations, les retrouve par
  recherche et les reprend là où tu les avais laissées. Chaque robot a sa couleur ; « Nouvel
  orchestrateur » ouvre un robot de plus pour mener deux chantiers en parallèle.
- **Mode agent.** Une tâche longue est découpée en secteurs confiés à des sous-agents (des
  mini-robots apparaissent autour de la tête), puis relue avant de t'être rendue.
- **Choisir ce que l'agent voit.** Menus des sous-agents, des serveurs MCP, des skills et du modèle,
  directement dans la bulle. Chaque case décochée est une vraie restriction pour l'agent.
- **Mode auto, avec garde-fou.** L'agent enchaîne sans te demander, mais les commandes dangereuses
  (suppressions récursives, `git push --force`, `git reset --hard`, formatage d'un disque, `DROP
  TABLE`…) sont bloquées et te sont soumises.
- **Mode présentation.** L'agent répond en slides HTML, ouvertes dans une visionneuse, avec un
  questionnaire final pour lui renvoyer tes décisions.
- **Thème clair, sombre ou celui de Windows**, choisi à l'installation et modifiable dans Réglages.
- **Interface en français ou en anglais**, par défaut celle de Windows, modifiable
  dans Réglages. Tout ce que LMClibot affiche ou envoie à l'agent suit ce choix.

## Claude Code et GitHub Copilot CLI

LMClibot n'a aucune intelligence propre et ne demande aucune clé d'API : il pilote un agent déjà
installé et connecté sur ton poste. Au premier lancement, il détecte les deux ; tu choisis ensuite
le moteur de chaque **nouvelle** session dans la fenêtre Sessions (bouton « Moteur »). Une session
garde toujours son moteur.

| | Claude Code | GitHub Copilot CLI |
|---|---|---|
| Bulle, sessions, reprise, Stop | oui | oui |
| Mode agent et sous-agents | oui | oui |
| Serveurs MCP ajoutés dans LMClibot | oui | oui |
| Skills | oui | oui : les siens, et tes skills Claude cochés |
| Mode auto avec garde-fou | oui | oui |
| Mode présentation | oui | oui |
| Dispatch vers le téléphone, catalogues de greffons, connecteurs claude.ai | oui | non (propres à Claude Code) |

## Prérequis

- Windows 10 ou 11 (64 bits). WebView2 est installé par l'installeur s'il manque.
- Au moins un des deux agents, installé et connecté à ton compte :
  - [Claude Code](https://docs.anthropic.com/claude-code) (compte Anthropic) ;
  - [GitHub Copilot CLI](https://docs.github.com/copilot) : `npm install -g @github/copilot`, puis
    `copilot login` (un abonnement Copilot, y compris gratuit, suffit).

## Installer

1. Télécharge `LMClibot-<version>-setup.exe` dans la dernière *Release* (installation sans droits
   administrateur), ou `LMClibot-<version>-portable.exe` pour l'exécuter sans installation.
   Télécharge aussi `SHA256SUMS.txt`, et `SHA256SUMS.txt.sig` si tu veux vérifier la signature.
2. Vérifie le fichier (voir ci-dessous).
3. Le binaire n'est pas encore signé par un certificat de code : Windows SmartScreen peut afficher
   « Windows a protégé votre ordinateur ». Clique sur *Informations complémentaires › Exécuter
   quand même* si la vérification est bonne.

## Vérifier ton téléchargement

### 1. L'empreinte (le fichier est intact)

Dans PowerShell, dans le dossier où tu as téléchargé les fichiers :

```powershell
$f = "LMClibot-1.1.1-setup.exe"   # le fichier téléchargé
$attendu = (Get-Content SHA256SUMS.txt | Where-Object { $_ -like "*  $f" }).Split(" ")[0]
(Get-FileHash $f -Algorithm SHA256).Hash -eq $attendu
```

`True` : le fichier est exactement celui qui a été publié. `False` : ne l'exécute pas, retélécharge-le.

### 2. La signature (il vient bien de l'auteur)

L'empreinte prouve que le fichier est intact, pas qui l'a publié. `SHA256SUMS.txt.sig` est une
signature Ed25519 de la version et de `SHA256SUMS.txt`, faite avec la clé de publication de
LMClibot. Pour la vérifier, il faut OpenSSL 3 (fourni avec
[Git pour Windows](https://git-scm.com/download/win)) :

```powershell
$v = "1.1.1"   # la version téléchargée, sans le « v »
"-----BEGIN PUBLIC KEY-----`nMCowBQYDK2VwAyEA/ocxSvM6vVUpPxIejFpi1EAH2dZd8srOz8jTXK95+Bo=`n-----END PUBLIC KEY-----" |
  Set-Content -Encoding ascii cle-lmclibot.pem
[byte[]]$m = [Text.Encoding]::ASCII.GetBytes("lmclibot $v`n") + [IO.File]::ReadAllBytes("$PWD\SHA256SUMS.txt")
[IO.File]::WriteAllBytes("$PWD\message.bin", $m)
[IO.File]::WriteAllBytes("$PWD\signature.bin", [Convert]::FromBase64String((Get-Content SHA256SUMS.txt.sig -Raw).Trim()))
& "C:\Program Files\Git\usr\bin\openssl.exe" pkeyutl -verify -pubin -inkey cle-lmclibot.pem -rawin -in message.bin -sigfile signature.bin
```

`Signature Verified Successfully` : les empreintes viennent bien de l'auteur, pour cette version.
Fais ensuite l'étape 1 : les deux ensemble prouvent que ton fichier est authentique.

## Mises à jour

LMClibot se met à jour tout seul : il vérifie les versions publiées ici au démarrage puis toutes
les 24 h, télécharge la nouvelle en arrière-plan et l'installe quand tu le fermes. La fenêtre
**À propos** (menu du robot ou de l'icône près de l'horloge) affiche ta version, propose
« Mettre à jour maintenant » et permet de couper les mises à jour automatiques.

L'app fait elle-même les deux vérifications ci-dessus avant toute mise à jour, avec la clé de
publication qu'elle embarque : une version dont la signature ne correspond pas est refusée.

## Sécurité

Chaque version est construite depuis les sources par une intégration continue, puis analysée par
Trivy (dépendances, binaire, secrets) et govulncheck (bibliothèque standard Go). Aucune version
n'est publiée avec une vulnérabilité HIGH ou CRITICAL corrigeable. La nomenclature logicielle
(SBOM CycloneDX) est jointe à chaque version.

LMClibot fonctionne localement et n'envoie aucune donnée à son auteur. Tes messages ne passent que
par l'agent que tu as choisi. Seule la vérification des mises à jour contacte github.com
(désactivable).

## Licence et mentions

Gratuiciel, tous droits réservés : voir `LICENCE.txt` dans chaque version. Composants tiers :
`THIRD-PARTY-NOTICES.txt`.

LMClibot est un outil indépendant, **non affilié à Anthropic ni à GitHub**, ni approuvé par eux.
« Claude » et « Claude Code » sont des marques d'Anthropic, PBC. « GitHub » et « GitHub Copilot »
sont des marques de GitHub, Inc.
