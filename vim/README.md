Vim
===

Commandes utiles
----------------

  ------------------ -----------------------------------------------------------------------------
  `daw` ou `caw`     Efface le mot du curseur, sans ou en passant en insert mode
  `$` ou `A`         Va à la fin de ligne, sans ou en passant en insert mode
  `0` ou `I`         Va au début de la ligne, sans ou en passant en insert mode
  `yy`               Copie la ligne courante
  `y$`               Copie la ligne depuis le curseur jusqu'a la fin de la ligne
  `p` ou `P`         Colle la ligne courante apres ou avant le curseur
  `:f` ou `ctrl-g`   Affiche le nom du fichier
  `:new`             Ouvre une nouvelle fenetre
  `0D` or `^D`       Efface le contenu de toute la ligne ou depuis le premier caractere non vide
  ------------------ -----------------------------------------------------------------------------

Substitutions
-------------

  --------------------------------------------------------------------------------------------- ---------------------------------------------------------------------------
  `:%s/foo/bar/g` & Substitue foo a bar pour toutes les lignes et pour toutes les occurrences   Substitue foo a bar pour la ligne courante et pour toutes les occurrences
  `:s/foo/bar/gc`                                                                               Comme precedemment mais demande confirmation
  `:%s/\<foo\>/bar/gc` & Comme precedemment mais match seulement les *mots* foo                 
  --------------------------------------------------------------------------------------------- ---------------------------------------------------------------------------

Note: in the first line, `\s` finds is for whitespaces, `\+` is for one
or more occurrence of previous pattern and `$` is for the end of the
line

Affichage lignes/colonnes
-------------------------

Si les lignes et les colonnes n'apparaissent pas par defaut, il faut
ajouter dans le fichier `.vimrc`:

    set ruler

Reglages indentation (pour Python)
----------------------------------

Dans le fichier `.vimrc`, ajouter les lignes suivantes:

    syntax on
    filetype indent plugin on

    set tabstop=8
    set expandtab
    set shiftwidth=4
    set softtabstop=4

Les deux premieres lignes permettent d'activer la coloration syntaxique
et le plugin d'indentation.

La 3eme ligne permet de définir qu'une tabulation compte pour 8 colonnes
(à priori à ne pas changer!), la 4eme permet, lorsque la touche `TAB`
est activée, de générer le nombre approprié de tabulations, la 5eme
ligne indique le nombre de colonnes à identer lorsque une fonction
`reindent` est appelée, la dernière ligne définit le nombre de colonnes
d'une indentation.

Smart indent pour tout un fichier
---------------------------------

Pour indenter tout un fichier de manière propre (smart indent), il faut
taper:

    gg=G

`gg` positionne le curseur sur la premiere ligne, et `=G` reindente
depuis la position courante du curseur jusqu'à la fin du fichier (donnée
par `G`). Si on ne veut indenter qu'une seule ligne, taper `==`. Si on
veut indenter de la position du curseur actuelle jusqu'à la fin, taper
`=G`.

Attention, ne pas mettre de `:` ou de `/`.

Fortran
-------

A priori, `vim` considere que les fichiers fortran sont de types F77 et
veut donc que l'on commence à la colonne 7. Pour le forcer à afficher du
F90, il faut ajouter dans le `.vimrc`, juste avant le `syntax on\`:

    au BufRead,BufNewFile *.f90 let b:fortran_free_source=1

Do loops, F90
-------------

Par default, les do loops ne sont pas indentees pour ne pas \"abimer\"
les boucles du type `go to` ou les boucles avec label. Si on part du
principe que ces boucles n'existent pas dans les fichiers au format
`.f90`, il faut ajouter au fichier `.vimrc`:

    au BufRead,BufNewFile *.f90 let b:fortran_do_enddo=1

Ca permet d'indenter les do loops seulement pour les fichiers `.f90`.

Recherche de mots sans casse
----------------------------

Ajouter dans le fichier `.vimrc`:

    set ignorecase

Forcer la syntaxe pour un type de fichier
-----------------------------------------

Pour forcer les `.inc` a apparaitre comme du fortran, il faut ajouter
dans le fichier `.vimrc`:

    au BufRead,BufNewFile *.inc set filetype=fortran

Fichiers de configuration specifiques
-------------------------------------

Pour avoir certains reglages propres a un seul format de fichier pour
l'indentation, il faut creer un fichier un fichier:

    ~/.vim/after/indent/fortran.vim
    ~/.vim/after/indent/c.vim

Et ainsi de suite pour tous les langages de programmation.
