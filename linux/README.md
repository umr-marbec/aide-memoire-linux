Aide memoire for different linux things
================================================

# Bash

## Loops in bash

### Boucle sur les elements

Il est possible de boucler sur differents elements:

```
for v in 1 a toto
do
    echo $v
done
```

### Boucler sur les fichiers

```
for f in *txt
do 
  echo $f
done
```

### Boucle sur des indices 

Source: https://www.cyberciti.biz/faq/bash-for-loop/

Version start/end

```
#!/bin/bash
for i in {1..5}
do
   echo "Welcome $i times"
done
```

Version start/end/increment

```
#!/bin/bash
echo "Bash version ${BASH_VERSION}..."
for i in {0..10..2}
  do 
     echo "Welcome $i times"
 done
```


## Operations in bash

For simple operations:

```
num=$(($num1 + $num2))
```

**Warning: do not forget both brackets**


## Aliases dans des scripts shells

Les alias définis dans les fichiers `${HOME}/.bash_profile` et `${HOME}/.bashrc` ne sont pas reconnus lorsqu'ils sont appelés
d'un script. Pour que ce soit le cas, il faut taper au tout début du
script appelé:

```
shopt -s expand_aliases
. ~/.bash_aliases
```

## Variables d'environnement de commande

Quand une commande comme la suivante est appellee: 

    ./command -yes -no /home/username

differentes variables d'environnement sont automatiquement creees:

-   `$#`: `3` (le nombre d'arguments)
-   `$*` : `-yes -no /home/username` (tous les arguments sous forme de
    chaine de caracteres)
-   `$@` : `{"-yes", "-no", "/home/username"}` (tous les arguments mais
    sous forme de tableau)
-   `$0` : `./command`
-   `$1` : `-yes`
- `$2` : `-no`
- `$3` : `/home/username` (etc. pour autant d'arguments)
-   `$?` : `1` if error was occurred, else `0`

## Taille d'une variable 

Pour obtenir la taille d'une variable d'environnement (nombres de caracteres), rajouter un `#` devant:

    echo ${#HOME}

## Existence d'une variable d'environnent

Pour tester l'existence d'une variable d'environnement:

    if [ -z $HOME ]; then
        echo "HOME n'existe pas"
    fi

## Redirections

Pour rediriger les sorties standards d'une commande vers une sorte de
"poubelle", et de rediriger les erreurs au même endroit:

    commande > /dev/null 2>&1

Ici, on redirige les sorties de `commande` vers la "poubelle"
`/dev/null`, et on redirige l'erreur (donnée par la valeur `2`) au même
endroit que la sortie (donnée par la valeur `1`). Mais il faut le signe
`&` d'abord. A noter que la valeur `0` est l'entrée standard
(généralement le clavier).

Pour envoyer les erreurs dans un fichier, et les sorties dans un autre:

```
commande > std.txt 2>err.txt
```

## Scripts avec arguments

Pour recuperer des arguments à un script shell:

    while getopts :hd:n:r:u:m:j:e:s:v:t:k: V
    do
       echo $V
       echo ${OPTARG}
       case $V in
          (h) blabla
          (d) blabla
       esec
    done

Ici, les options sont `h`, `d`, `n`, etc. Les options suivies d'un signe
`:` attendent un argument, les autres non. La valeur de `$V` est celle
de l'option, tandis que la valeur de `${OPTARG}` est celle de
l'argument.\
A noter que le premier signe `:` (juste avant le h) permet de faire en
sorte que si une option censée avoir un argument n'en a pas, alors
`$V=:` et peut être capturée par un `(:)` pour afficher un message
d'erreur. Dans ce cas, lorsqu'une option est donnée mais n'existe pas,
alors `$V=?` et doit être capturée par `(\?)`.

## Lecture d'un fichier ligne par ligne

Pour lire un fichier ligne par ligne:

    while read ligne 
    do 
       commande 
    done < fichier

La variable `$ligne` contient le contenu de la ligne, et `fichier` est
le nom du fichier.

## `Here` scripts

Les \"Here scripts\" permettent de remplacer les arguments d'une
fonction par du texte entrer par l'utilisateur. Par exemple, la commande
`cat` qui permet d'afficher dans un terminal le contenu d'un fichier. Si
on veut remplacer le fichier d'entrée par un texte entré par
l'utilisateur:

    cat << EOF

    Ceci est un here script

    EOF

Cela reviendrait à ouvrir un fichier, entrer le texte dedans, et faire
un `cat fichier`. Ca peut permettre d'éviter de mettre des commandes
`echo` un peu partout. A noter que l'in peut ajouter des arguments
devant le `EOF`. Par exemple, `- EOF` permet d'enlever les tabulations.
