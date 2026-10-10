# 🥷 naruto-online-linux

Faire fonctionner le client OAS Games de **Naruto Online** sous Linux, **sans Windows**.

Ce projet est issu de [OS-fedora-gaming](https://github.com/ghermetz/OS-fedora-gaming). Il fonctionne sur toute distribution qui a Wine (Fedora, Ubuntu/Debian, Arch, openSUSE).

**EN TL;DR:** The official Windows launcher (.NET + embedded Chromium 75 + PPAPI Flash) runs fine under plain **Wine** with **.NET 4.8**. No rendering hacks are needed. One script sets everything up. Bring your own game files: nothing copyrighted is hosted here.

> ✅ **Validé le 10/10/2026** sur Fedora 43 KDE avec Wine 11 Staging : le launcher démarre, la connexion fonctionne, le jeu Flash charge à 100 % et l'écran de jeu répond.

## Installation

Il te faut Wine 9 ou plus récent et winetricks. Le script les installe s'ils manquent (dnf, apt, pacman ou zypper).

```bash
git clone https://github.com/ghermetz/naruto-online-linux.git
cd naruto-online-linux
./naruto-online.sh setup "/chemin/vers/Naruto Online"   # une fois, ~30 min
```

`/chemin/vers/Naruto Online` est le dossier du launcher qui contient `Naruto Online.exe`. Sous Windows, c'est `C:\Program Files (x86)\Naruto Online`.

Le script fait quatre choses :
1. Il copie le jeu dans `~/Games/Naruto Online`. Lancé depuis une partition NTFS, le jeu est lent et provoque des verrous de fichiers.
2. Il crée un préfixe Wine dédié dans `~/.local/share/wineprefixes/naruto-online`.
3. Il installe **.NET Framework 4.8** et `corefonts` avec winetricks. C'est l'étape longue.
4. Il ajoute **« Naruto Online »** au menu des applications.

## Jouer

Lance le jeu depuis le menu des applications, ou avec :

```bash
./naruto-online.sh run
```

## Dépannage

| Symptôme | Solution |
|---|---|
| Le chargement reste bloqué ou la page est blanche | `./naruto-online.sh clear-cache`, puis relance le jeu |
| « Préfixe incomplet » | Relance `setup` : les étapes déjà faites sont sautées |
| Tout recommencer | `./naruto-online.sh uninstall`, puis `setup` |
| Voir les erreurs Wine | `WINEDEBUG=err+all ./naruto-online.sh run` |

## ⚠️ Fausses pistes, à ne pas suivre

- **`WINEDLLOVERRIDES="d3d11,dxgi=d"`**, souvent présenté comme le remède à « l'écran noir », **fait planter le launcher**. `libcef.dll` dépend de ces DLL, et l'erreur obtenue est `Could not load file or assembly 'CefSharp.Core.dll'`.
- **Ruffle** : le jeu est une grosse application AS3 modulaire qui se connecte en socket aux serveurs, ce qui dépasse ce que Ruffle sait faire de façon fiable.
- **Bottles (Flatpak)** : il n'apporte rien, et son bac à sable complique l'usage de winetricks.

En dernier recours, tu peux utiliser une VM Windows (WinBoat, VirtualBox…) avec le launcher natif.

## ⚖️ Note légale

Ce dépôt ne contient **ni le jeu, ni le launcher, ni de contenu Naruto**, seulement un script et de la documentation. Tu dois fournir ton propre dossier de jeu. Naruto Online est une marque de ses propriétaires respectifs, et ce projet n'y est pas affilié.
