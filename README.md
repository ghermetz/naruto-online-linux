# 🥷 naruto-online-linux

**Jouer à Naruto Online sous Linux avec le launcher officiel d'OAS Games, sans Windows.**

> 🇫🇷 **Résumé** — Le launcher Windows officiel (application .NET avec Chromium 75 et Flash intégrés) fonctionne avec **Wine** et **.NET 4.8**, sans aucun réglage de rendu. Un seul script installe tout, à partir de l'installeur officiel du jeu. Ce dépôt ne contient ni le jeu ni le launcher.
>
> 🇬🇧 **Summary** — The official Windows launcher (.NET app with embedded Chromium 75 and Flash) runs under plain **Wine** with **.NET 4.8**, with no rendering tweaks. One script sets everything up from the game's official installer. Nothing copyrighted is hosted here. The rest of the documentation is in French.

Ce projet est issu de [OS-fedora-gaming](https://github.com/ghermetz/OS-fedora-gaming) et fonctionne sur toute distribution qui a Wine (Fedora, Ubuntu/Debian, Arch, openSUSE).

> ✅ **Validé le 10/10/2026** sur Fedora 43 KDE avec Wine 11 Staging, dans une VM avec et sans accélération 3D : le launcher démarre, la connexion fonctionne, le jeu charge à 100 %, le son et la souris fonctionnent.

## Installation

Il vous faut Wine 9 ou plus récent et winetricks. Le script les installe s'ils manquent (dnf, apt, pacman ou zypper).

1. Téléchargez l'**installeur officiel** sur le site du jeu (par exemple `Naruto Online_fr_v3.4.7.0.zip`).
2. Lancez :

```bash
git clone https://github.com/ghermetz/naruto-online-linux.git
cd naruto-online-linux
./naruto-online.sh setup ~/Téléchargements/"Naruto Online_fr_v3.4.7.0.zip"   # une fois, ~30 min
```

La source peut être :
- l'installeur officiel, en archive `.zip` ou directement en `.exe` ;
- le dossier du jeu déjà installé sous Windows, qui contient `Naruto Online.exe` (par défaut `C:\Program Files (x86)\Naruto Online`).

Sans argument, `./naruto-online.sh setup` cherche tout seul dans vos Téléchargements et sur une partition Windows montée.

Le script fait quatre choses :
1. Il crée un préfixe Wine dédié dans `~/.local/share/wineprefixes/naruto-online`.
2. Il installe **.NET Framework 4.8** et `corefonts` avec winetricks. C'est l'étape longue.
3. Il installe le jeu dans `~/Games/Naruto Online` : installation silencieuse de l'installeur officiel, ou copie du dossier Windows (lancé depuis une partition NTFS, le jeu est lent et provoque des verrous de fichiers).
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
| Le chargement reste bloqué ou la page est blanche | `./naruto-online.sh clear-cache`, puis relancez le jeu (il faudra vous reconnecter) |
| « Préfixe incomplet » | Relancez `setup` : les étapes déjà faites sont sautées |
| Tout recommencer | `./naruto-online.sh uninstall`, puis `setup` |
| Voir les erreurs Wine | `WINEDEBUG=err+all ./naruto-online.sh run` |

## ⚠️ Fausses pistes, à ne pas suivre

- **`WINEDLLOVERRIDES="d3d11,dxgi=d"`**, souvent présenté comme le remède à « l'écran noir », **fait planter le launcher** : `libcef.dll` dépend de ces DLL, et l'erreur obtenue est `Could not load file or assembly 'CefSharp.Core.dll'`.
- **Ruffle** : le jeu est une grosse application ActionScript 3 en dizaines de modules, qui se connecte en socket aux serveurs. C'est au-delà de ce que Ruffle sait faire de façon fiable.
- **Bottles (Flatpak)** : il n'apporte rien ici, et son bac à sable complique l'usage de winetricks.

**Rendu graphique** : quand Direct3D 11 n'est pas disponible via Wine, le Chromium du launcher bascule tout seul sur un autre mode de rendu, sans écran noir. Le cas d'une carte graphique récente, où Direct3D 11 est disponible, n'a pas encore été testé sur du vrai matériel. Si vous obtenez un écran noir, ouvrez une *issue*.

En dernier recours, une VM Windows (WinBoat, VirtualBox…) avec le launcher natif fonctionne toujours.

## Contribuer

Les retours sur d'autres distributions et sur du vrai matériel (AMD, Intel, NVIDIA) sont les bienvenus dans les *issues*.

## ⚖️ Note légale

Ce dépôt ne contient **ni le jeu, ni le launcher, ni aucun contenu Naruto** : seulement un script et de la documentation. Vous fournissez l'installeur officiel du jeu. Naruto Online est une marque de ses propriétaires respectifs, et ce projet n'y est pas affilié.
