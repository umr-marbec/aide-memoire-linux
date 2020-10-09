# Langage C

## Fonction qui sort un tableau

Une possibilité pour ressortir un tableau d'une fonction est d'utiliser
une variable pointer `static`. Le fait que la variable soit statique
permet qu'elle garde sa valeur entre deux appels successifs.

## Pointeurs

Si on définit comme en tete du fichier:

    int x, *p;

Alors `p` est un pointeur, et `*p` est ce que référence `p` Si `x` est
une variable quelconque, `&x` est l'adresse de `x`. Ainsi `&x` est un
pointeur.

## Typedef et pointeurs de fonction

Si on écrite, dans un fichier `.h`:

    typedef type_output (*mypointer) (type1 arg1, type2 arg2);

On définit ainsi un type de variable `mypointer` qui serait de type de
\"pointeurs de fonction\". On est obligé de définir à la fois le type de
sortie de la fonction pointée (ici, `type_output` ainsi que ses
arguments (et leurs types, ici `type1 arg1, type2 arg2`).

## Enumerations

Si dans un fichier .h on écrit:

    typedef enum mois_ {Jan, Feb, Mar} mois;

Alors on définit une structure `mois`, dont les valeurs sont `Jan=0`,
`Fev=1`, `Mar=2`. Ce qui permet de faire simplement de l'arithmétique de
`mois`, à savoir:

    toto = Jan + Mar /* = 2 */

## Tableaux/Pointeurs de pointeurs

Un tableau peut être vu comme un pointeur vers une zone mémoire où se
succèdent des éléments d'un même type. Si on veut définir une fonction
`trace`, qui calcule la trace d'une matrice $3\times3$, on peut la
définir de manière équivalence par:

    int trace(int mat[][3])

et

    int trace(int (* mat)[3])

Ici, mat est déclarée comme un tableau de pointeurs sur des tableaux à 3
entiers.\
Si on écrit int `tab[5]`, on définit `tab` comme un pointeur vers le
premier élément du tableau: `tab = &tab[0]`.

## Conditions simplifiées

Pour faciliter l'utilisation de conditions:

    variable condition ? A:B

Si `condition` est verifiee (True), alors `variable=A` et sinon
`variable=B`.

## Iterations

Si on veut soustraire une variable `B` à une variable `A`, on peut
utiliser la commande:

    A=A-B

Mais cette commande va être ralentie car on va accéder 2 fois à la
mémoire relative à `A`. Il vaut mieux privilégier:

    A-=B // A = A-B
    A+=B // A = A+B
    A*=B // A = A*B

## Lecture d'un fichier caractères par caractères

Si on a un script comme ceci:

```c
    int main()
    {
      FILE *fp;
      int ch;
      
      fp=fopen("./test.txt", "r");
      
      while((ch = fgetc(fp)) != 'y')
        printf("=====%c\n", ch);
     
      ungetc(ch, fp);

      while((ch = fgetc(fp)) != EOF)
        printf("=====%c\n", ch);

      fclose(fp)
    }
```

On lit d'abord un fichier texte (lecture seule), que l'on stocke comme
un pointeur vers un fichier. Ensuite, on fait sur la variable `ch`, qui
est un entier.

On détermine sa valeur grace à la commande `fgetc`, qui lit la valeur du
*premier* caractère, la stocke dans la variable `ch` et ensuite va
\"pointer\" vers le caractère suivant. Dès que le caractère lu est un
`y`, alors on sort de la boucle while. A ce moment, la valeur de `ch`
correspond donc au caractère `y`. Si on poursuit avec la deuxième
boucle, on va constater que le caractère `y` ne sera pas affiché. Ce qui
est normal, puisque lors de la lecture du `y` via `fgetc`, le
\"curseur\" de lecture s'est déplacé sur le caractère d'après. Pour
revenir au `y`, il faut utiliser la commande `ungetc(ch, fp)`.
