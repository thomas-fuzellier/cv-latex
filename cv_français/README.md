# CV LaTeX — Thomas Fuzellier

CV d'une page A4 en deux colonnes : une colonne gauche bleu foncé (photo, contact, compétences, langues, centres d'intérêt) et une colonne droite blanche (en-tête, accroche, expériences, formation, projets). Il existe une version française et une version anglaise.

## Contenu du dossier

```
CV/
├── cv.tex            Contenu du CV en français  -> c'est lui que tu modifies
├── cv-en.tex         Contenu du CV en anglais (normes anglophones)
├── cv-style.sty      Design commun : couleurs, tailles, marges, commandes
├── photo.jpg         Photo de profil
├── README.md         Ce fichier
└── .vscode/
    └── settings.json Configuration de LaTeX Workshop (compilation pdflatex)
```

Le principe est simple : **le contenu est dans les `.tex`, le design est dans `cv-style.sty`**. Une modification du design s'applique automatiquement aux deux versions.

---

## 1. Installation (une seule fois)

1. Installe **MiKTeX**.
2. Ouvre **MiKTeX Console**, va dans *Settings* et règle « Install missing packages » sur **Always**. Ainsi, les paquets nécessaires (`opensans`, `fontawesome5`, `babel-french`…) s'installent automatiquement à la première compilation.
3. Dans **VS Code**, installe l'extension **LaTeX Workshop**. Le fichier `.vscode/settings.json` la configure déjà pour utiliser pdflatex.

## 2. Générer le PDF

Dans VS Code, ouvre `cv.tex` (ou `cv-en.tex`), puis clique sur ▶ *Build LaTeX project*.

Tu peux aussi compiler depuis le terminal de VS Code (*Terminal > New Terminal*) :

```powershell
pdflatex cv.tex       # -> cv.pdf    (français)
pdflatex cv-en.tex    # -> cv-en.pdf (anglais)
```

La première compilation est plus longue, le temps que MiKTeX télécharge les paquets manquants.

---

## 3. Modifier le contenu (`cv.tex`)

Chaque rubrique est commentée dans le fichier. Voici les commandes disponibles.

### Colonne droite

```latex
\cvheader{Prénom}{NOM}{Intitulé du poste}
\cvsummary{Texte de l'accroche}

\cvsection{Expériences}
\begin{cvtimeline}
  \cvexperience{Titre}{Dates}{Entreprise}{Lieu}{Contexte en une ligne}
  \begin{cvitems}
    \item Une mission avec un \textbf{mot-clé} en gras
  \end{cvitems}
  \cvsep                                  % espace avant l'entrée suivante
  ...
\end{cvtimeline}

\cveducation{Diplôme}{Dates}{Établissement}{Lieu}{Détails}
\cvproject{Titre}{Date}{Lien ou lieu}{Technologies}{Description}
```

### Colonne gauche

```latex
\cvphoto{photo.jpg}
\sidesection{Titre de rubrique}
\sidecontact{\faPhone}{texte}             % contact avec icône
\sidegroup{Catégorie}{élément, élément}   % compétences, loisirs
\sideitem{Libellé}{Valeur}                % langues
```

Icônes disponibles : `\faEnvelope`, `\faPhone`, `\faGlobe`, `\faMapMarker`, `\faCar` et `\faFlag`. Le paquet *fontawesome5* en propose d'autres.

### Opérations courantes

| Je veux… | Comment |
|---|---|
| Ajouter une expérience | Copier un bloc `\cvexperience … \end{cvitems}` et le placer **avant** le premier `\cvsep` pour qu'il apparaisse en tête |
| Masquer une ligne sans la supprimer | Mettre un `%` au début de la ligne |
| Réordonner les rubriques | Déplacer un bloc entier, de `\cvsection{…}` jusqu'à son `\end{cvtimeline}` |
| Activer la rubrique *Qualités* | Retirer les `%` du bloc « Qualités » dans la colonne gauche |
| Ajouter un lien cliquable | `\href{https://…}{texte affiché}` |

### Pièges LaTeX

| Écrire | Pour obtenir |
|---|---|
| `\&` `\%` `\#` `\_` `\$` | & % # _ $ |
| `--` | un tiret de date (–) |
| `~:` | un deux-points jamais rejeté en début de ligne |
| `\textbf{…}` | du gras |

---

## 4. Modifier le design (`cv-style.sty`)

Tous les réglages se trouvent dans les **sections 2 (couleurs) et 3 (dimensions)** du fichier.

### Couleurs (codes hexadécimaux)

| Réglage | Élément |
|---|---|
| `cvsidebar` | Fond de la colonne gauche |
| `cvsidelabel` | Libellés et icônes de la colonne gauche |
| `cvdark` | Titres de la colonne droite |
| `cvaccent` | Entreprises, lieux, timeline, puces |
| `cvgray` | Dates et accroche |

### Mise en page

| Réglage | Effet |
|---|---|
| `\cvsidebarwidth` | Largeur de la colonne bleue |
| `\cvbody` | Taille du texte de la colonne droite (`{8.9}{10.8}` = taille / interligne) |
| `\cvsidesize` | Taille du texte de la colonne gauche |
| `\cvsecskip` | Espace avant chaque titre de rubrique (droite) |
| `\cventryskip` | Espace entre deux expériences |
| `\cvsidesecskip` | Espace minimum avant chaque titre (gauche) |
| `top=…` / `bottom=…` (ligne `geometry`) | Marges haute et basse de la page |

### Photo

| Réglage | Effet |
|---|---|
| `\cvphotoside` | Taille du cadre (4,4 cm maximum) |
| `\cvphotowidth` | Largeur de l'image, à garder environ 0,3 cm au-dessus de `\cvphotoside` |
| `\cvphotocorner` | Forme : `0pt` carré, `5pt` coins arrondis, moitié de `\cvphotoside` pour un rond |
| `\cvphotoyshift` / `\cvphotoxshift` | Recentrer le visage dans le cadre (valeur positive = image remontée) |
| `\cvsidetop` | Décaler toute la colonne gauche (valeur négative = vers le haut) |

> La colonne gauche a une hauteur fixe : ses rubriques se répartissent automatiquement sur toute la page. Agrandir la photo resserre donc les espaces entre les rubriques, sans rien faire déborder.

### Options du paquet

Elles se placent dans la ligne `\usepackage[...]{cv-style}` en haut du `.tex` :

| Option | Effet |
|---|---|
| `helvet` | Police Helvetica au lieu d'Open Sans |
| `noicons` | Sans les icônes de contact |
| `nobabel` | Sans la césure française (utilisé par `cv-en.tex`) |

---

## 5. Version anglaise (`cv-en.tex`)

Le design est le même, mais le contenu suit les usages des CV anglophones :
- **Pas de photo, de nationalité ni de permis**, une pratique courante au Royaume-Uni et aux États-Unis. Ces lignes sont commentées et peuvent être réactivées, par exemple pour un poste en défense qui exige la nationalité.
- **Verbes d'action** au présent pour le poste actuel, au passé pour les précédents.
- **Équivalences des diplômes** (Diplôme d'Ingénieur → MSc in Engineering).
- **Orthographe britannique** et césure anglaise.

Quand tu modifies une expérience dans `cv.tex`, pense à reporter la modification dans `cv-en.tex`.

---

## 6. Si quelque chose ne va pas

| Problème | Solution |
|---|---|
| Le CV passe sur 2 pages | Raccourcir une puce, ou réduire légèrement `\cvbody` ou `\cvsecskip` |
| Une puce se coupe sur deux lignes avec un mot coupé | La reformuler un peu plus court |
| « pdflatex introuvable » | Redémarrer VS Code, ou Windows, après l'installation de MiKTeX |
| Erreur de compilation | Ouvrir `cv.log` et chercher la première ligne commençant par `!` |
| Un paquet manque | Vérifier que MiKTeX est bien réglé sur « Install missing packages: Always » |

**Conseil :** après chaque modification, recompile et vérifie que tout tient sur une page et qu'aucun mot n'est coupé en fin de ligne.