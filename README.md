# 🥷 naruto-online-linux

**Jouer à Naruto Online sous Linux avec le launcher officiel d'OAS Games, sans Windows.**

> 🇫🇷 **Résumé** — Le launcher Windows officiel (application .NET avec Chromium 75 et Flash intégrés) fonctionne avec **Wine** et **.NET 4.8**, sans aucun réglage de rendu. Un seul script installe tout, à partir de l'installeur officiel du jeu. Ce dépôt ne contient ni le jeu ni le launcher.
>
> 🇬🇧 **Summary** — The official Windows launcher (.NET app with embedded Chromium 75 and Flash) runs under plain **Wine** with **.NET 4.8**, with no rendering tweaks. One script sets everything up from the game's official installer. Nothing copyrighted is hosted here. The rest of the documentation is in French.

Ce projet est issu de [OS-fedora-gaming](https://github.com/ghermetz/OS-fedora-gaming) et fonctionne sur toute distribution qui a Wine (Fedora, Ubuntu/Debian, Arch, openSUSE).

> ✅ **Validé le 10/10/2026** sur Fedora 43 KDE avec Wine 11 Staging, dans une VM avec et sans accélération 3D : le launcher démarre, la connexion fonctionne, le jeu charge à 100 %, le son et la souris fonctionnent.
>
> ✅ **Testé le 12/10/2026 sur un vrai PC** (Fedora 44, Wine 11 Staging, Radeon RX 7600 XT) avec l'installeur officiel 3.4.7.0 : installation complète, launcher et connexion avec le rendu graphique accéléré. Le chargement du jeu s'est arrêté à 14 % parce qu'un serveur du jeu refusait les connexions de ce réseau (voir *Dépannage*).

## Installation

Il vous faut Wine 9 ou plus récent et winetricks. Le script les installe s'ils manquent (dnf, apt, pacman ou zypper).

1. Téléchargez l'**installeur officiel** sur le site du jeu (par exemple `Naruto Online_fr_v3.4.7.0.zip`).
2. Lancez :

```bash
git clone https://github.com/ghermetz/naruto-online-linux.git
cd naruto-online-linux
./naruto-online.sh setup ~/Téléchargements/"Naruto Online_fr_v3.4.7.0.zip"   # une fois, 5 à 30 min
```

La source peut être :
- l'installeur officiel, en archive `.zip` ou directement en `.exe` ;
- le dossier du jeu déjà installé sous Windows, qui contient `Naruto Online.exe` (par défaut `C:\Program Files (x86)\Naruto Online`).

Sans argument, `./naruto-online.sh setup` cherche tout seul dans vos Téléchargements et sur une partition Windows montée.

Le script fait quatre choses :
1. Il crée un préfixe Wine dédié dans `~/.local/share/wineprefixes/naruto-online`.
2. Il installe **.NET Framework 4.8** et `corefonts` avec winetricks. C'est l'étape longue (5 minutes sur un PC récent avec la fibre, jusqu'à 30 minutes ailleurs).
3. Il installe le jeu. Avec l'installeur officiel, **une fenêtre s'ouvre : cliquez sur « Utiliser »**. L'installeur ne sait plus s'installer sans fenêtre (version 3.4.7.0) et choisit lui-même son dossier dans le préfixe Wine ; le script le relie à `~/Games/Naruto Online`. À la fin, le launcher s'ouvre tout seul ; fermez-le pour que le script termine. Avec un dossier Windows, le jeu est copié dans `~/Games/Naruto Online` (lancé depuis une partition NTFS, il est lent et provoque des verrous de fichiers).
4. Il ajoute **« Naruto Online »** au menu des applications.

## Jouer

Lancez le jeu depuis le menu des applications, ou avec :

```bash
./naruto-online.sh run
```

Le premier chargement du jeu est long, comme le signale le launcher lui-même.

**Variante « tout-en-un »** : `./naruto-online.sh launch` installe au premier appel (en cherchant l'installeur, ou en demandant où il se trouve si `kdialog` est disponible), puis lance le jeu les fois suivantes. C'est pratique pour une icône de menu.

## Dépannage

| Symptôme | Solution |
|---|---|
| Le chargement du jeu reste bloqué (par exemple à 14 %) et recommence | Le jeu n'arrive pas à joindre un de ses serveurs : `debug.log`, dans le dossier du jeu, contient alors `IO_ERROR`. Vérifiez si le même serveur charge sous Windows ou sur un autre réseau ; essayez un autre serveur de jeu |
| La page reste blanche | `./naruto-online.sh clear-cache`, puis relancez le jeu (il faudra vous reconnecter) |
| « Préfixe incomplet » | Relancez `setup` : les étapes déjà faites sont sautées |
| Tout recommencer | `./naruto-online.sh uninstall`, puis `setup` |
| Voir les erreurs Wine | `WINEDEBUG=err+all ./naruto-online.sh run` |

## ⚠️ Fausses pistes, à ne pas suivre

- **`WINEDLLOVERRIDES="d3d11,dxgi=d"`**, souvent présenté comme le remède à « l'écran noir », **fait planter le launcher** : `libcef.dll` dépend de ces DLL, et l'erreur obtenue est `Could not load file or assembly 'CefSharp.Core.dll'`.
- **Ruffle** : le jeu est une grosse application ActionScript 3 en dizaines de modules, qui se connecte en socket aux serveurs. C'est au-delà de ce que Ruffle sait faire de façon fiable.
- **Bottles (Flatpak)** : il n'apporte rien ici, et son bac à sable complique l'usage de winetricks.

**Rendu graphique** : quand Direct3D 11 n'est pas disponible via Wine, le Chromium du launcher bascule tout seul sur un autre mode de rendu, sans écran noir. Sur une vraie carte graphique (Radeon RX 7600 XT), Direct3D 11 via Wine fonctionne : le launcher et la page du jeu s'affichent normalement. Si vous obtenez un écran noir, ouvrez une *issue*.

En dernier recours, une VM Windows (WinBoat, VirtualBox…) avec le launcher natif fonctionne toujours.

## Contribuer

Les retours sur d'autres distributions et sur du vrai matériel (AMD, Intel, NVIDIA) sont les bienvenus dans les *issues*.

## ⚖️ Note légale

Ce dépôt ne contient **ni le jeu, ni le launcher, ni aucun contenu Naruto** : seulement un script et de la documentation. Vous fournissez l'installeur officiel du jeu. Naruto Online est une marque de ses propriétaires respectifs, et ce projet n'y est pas affilié.
