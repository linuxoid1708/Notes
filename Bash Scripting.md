
# Special variables


## Variables liées aux arguments

```bash
$#       => nombres arguments
$0       => le nom du script
$1,$2,$3 => les arguments individuels
-------------------------------------------------------------------------------- 
$*       => Tous les arguments sous forme d'une chaine
$@       => Tous les arguments mais sous forme de tableau 
```

## Variables liées à l'exécution

```bash
$?       => Code de retour de la derniere commande
$$       => PID du script en cours
$!       => PID du dernier processus en arrière-plan
$_       => Le dernier argument de la derniere commande
```

## Gestion des erreurs et signaux

```bash
$BASH_SOURCE  => le nom du script source , utilisé dans les scripts sourcés
$LINENO       => Affiche le numéro de ligne actuel dans le script
$SECONDS      => Nombre de secondes écoulées depuis le début du script
```


# Declaration des variables 

Dans un cadre global

```bash
global_var="Variable globale"
myvar=123

current_date=$(date)
user=$(whoamo)

files=$(ls)

------------------------SUBSHELL---------------------------------------------

output=$(echo "Ce script tourne dans un subshell")
echo "$output"

pwd (cd /tmp && echo "Inside tmp : $(pwd)") pwd # Affichera toujours le répertoire précédent
```

Dans une fonction 

```bash
global_var="Variable globale"

my_function(){
     local local_var="Variable locale"
     echo "Dans la fonction : $local_var"
     echo "Global acccessible dans la fonction : $global_var"
}

echo "Avant la fonction : $global_var" # Variable global
my_function       # Variable local
				  # Variable Global
echo "Apres la fonction : $global_var" # Variable globale
```


# Variables de tableau 

## Tableau 


**Tableau classique :** 

| my_array=("v1" "v2") | Déclare un tableau classique avec deux valeurs.      |
| -------------------- | ---------------------------------------------------- |
| ${my_array[0]}       | Accède au 1er élément du tableau.                    |
| ${my_array[@]}       | Affiche tous les éléments du tableau.                |
| ${#my_array[@]}      | Retourne le nombre total d'éléments dans le tableau. |
| ${!my_array[@]}      | Retourne tous les **index** du tableau.              |
| unset my_array[1]    | Supprime l'élément à l'index 1.                      |
|                      |                                                      |

**Tableau associatif :**


| declare -A my_assoc_array    | Déclare un tableau associatif.                           |
| ---------------------------- | -------------------------------------------------------- |
| my_assoc_array["clé1"]="v1"  | Associe la clé **`clé1`** à la valeur v1                 |
| ${my_assoc_array["clé1"]}    | Accède à la valeur associée à la clé clé1                |
| ${!my_assoc_array[@]}        | Affiche toutes les clés du tableau associatif.           |
| ${#my_assoc_array[@]}        | Retourne le nombre de paires clé/valeur dans le tableau. |
| unset my_assoc_array["clé1"] | Supprime la paire clé/valeur associée à la clé clé1      |


**Commandes liées aux Tableaux** 

| my_array[index]           | Affecte une valeur à un index spécifique dans un tab class                                 |
| ------------------------- | ------------------------------------------------------------------------------------------ |
| ${my_array[@]}            | Retourne **tous les éléments** du tableau.                                                 |
| ${my_array[*]}            | Similaire à `${my_array[@]}`. Utilisé dans certaines boucles.                              |
| ${my_array[indice]}       | Retourne l’élément à l’index **`indice`**.                                                 |
| ${#my_array[@]}           | Donne le **nombre total** d’éléments dans un tableau classique.                            |
| ${!my_array[@]}           | Retourne tous les **index disponibles** dans le tableau classique.                         |
| unset my_array[index]     | Supprime l’élément à un index particulier.                                                 |
| declare -A my_assoc_array | Déclare un **tableau associatif** (clé/valeur).                                            |
| ${my_assoc_array[clé]}    | Accède à une valeur associée à une clé spécifique dans un tableau associatif.              |
| ${!my_assoc_array[@]}     | Renvoie toutes les **clés** dans le tableau associatif.                                    |
| ${#my_assoc_array[@]}     | Retourne le **nombre total de paires clé/valeur** dans le tableau associatif.              |
| ${my_array[@]}            | Chaque élément est traité individuellement, idéal pour les boucles.                        |
| ${my_array[*]}            | Retourne tous les éléments **en une seule chaîne**, séparés par l'IFS (par défaut espace). |
| declare -a                | Déclare un **tableau classique**. Par défaut, non obligatoire pour les tableaux simples.   |
| declare -r                | Rend une variable **en lecture seule** (immuable).                                         |
| declare -x                | **Exporte** une variable dans l’environnement des processus enfants.                       |


```bash
#Delcaration de tableau
my_array=("valeur1" "valeur2" "valeur3")

#Acceder à un element du tableau
echo "${my_array[0]}" # valeur1
echo "${my_array[1]}" # valeur2

#Ajouter ou modifer ou supprimer un element
my_array[3]="valeur4" 
my_array[1]="nouvelle_valeur2"
unset my_array[1] # supprime l'element 1 

# NOMBRES D'ELEMENTS

echo "Nombre d'éléments : ${#my_array[@]}"

# LONGEUR D'UN ELEMENT EN PARTICULIER

echo "${#my_array[0]}"  # valeur1 => retourne le nombre de caracteres

!my_array[@]        
```

# Itération sur un tableau 

```bash
for element in "${my_array[@]}"; do
	echo "Element : $element"
done


---------iterer_avec_les_index-----------------------

for i in "${!my_array[@]}"; do
	echo "Index : $i, Valeur : ${my_array[$i]}"
done

!my_array[@] => # Renvoie tous les index du tableau
my_array[$i] => # Accède à la valeur associée à chaque index

```

# Tableau associatif

Utilisation de clés plutôt que index numériques

```bash
declare -A my_assoc_array

my_assoc_array["Clef1"]="Valeur1"
my_assoc_array["Clef2"]="Valeur2"

-------Acceder_aux_elements-----------------------

echo "${my_assoc_array["Clef1"]}" # affiche Valeur1

-------Parcourir_tableau_associatif----------------

for key in "${!my_assoc_array[@]}"; do
	echo "Clé : $key , Valeur : ${my_assoc_array[$key]}"
done


```

# Manipuler des tableaux 

```bash
unset my_array[1]                => supprime un element
unset my_array                   => supprime le tableau
copier_array =("${my_array[@]}") => copie le tableau
```


# Syntaxes de conditions 

| **Syntaxe** | **Nom** | **Description** | **Utilisation** | **Exemple** |     |
| ----------- | ------- | --------------- | --------------- | ----------- | --- |

|               |                         |                                                                           |                                                         |                     |
| :-----------: | :---------------------: | :-----------------------------------------------------------------------: | :-----------------------------------------------------: | :-----------------: |
| **`[ ... ]`** | **Test simple / POSIX** | Exécute des tests conditionnels basiques (numériques, chaînes, fichiers). | Comparaisons arithmétiques, chaînes, tests de fichiers. | `[ "$a" -eq "$b" ]` |

|                 |                      |                                                                                            |                                                    |                        |
| :-------------: | :------------------: | :----------------------------------------------------------------------------------------: | :------------------------------------------------: | :--------------------: |
| **`[[ ... ]]`** | **Test avancé Bash** | Amélioration de **`[ ]`**, supporte les expressions régulières et plus de tests complexes. | Comparaisons avancées et vérifications de chaînes. | `[[ "$a" == "abc*" ]]` |

|                 |                             |                                                    |                                                                      |               |
| :-------------: | :-------------------------: | :------------------------------------------------: | :------------------------------------------------------------------: | :-----------: |
| **`(( ... ))`** | **Expression arithmétique** | Évalue des expressions arithmétiques (comme en C). | Calculs et tests arithmétiques sans guillemets autour des variables. | `(( a < b ))` |

|               |              |                                                                                |                                                                                  |                 |
| ------------- | :----------: | :----------------------------------------------------------------------------: | :------------------------------------------------------------------------------: | :-------------: |
| **`( ... )`** | **Subshell** | Exécute une commande ou un ensemble de commandes dans un nouveau **subshell**. | Exécute des commandes dans un environnement isolé (indépendant du shell parent). | `(cd /tmp; ls)` |

|                 |                       |                                                                      |                                                                         |                                   |
| :-------------: | :-------------------: | :------------------------------------------------------------------: | :---------------------------------------------------------------------: | :-------------------------------: |
| **`{ ... ; }`** | **Bloc de commandes** | Regroupe plusieurs commandes à exécuter dans l'environnement actuel. | Exécute un ensemble de commandes dans l'environnement du shell courant. | `{ echo "Hello"; echo "World"; }` |

## Opérateurs  avec [] et [[]]

**Comparaison numérique**

| Operateur | Siginifaction       | Utilisation avec [] | Utilisation avec [[]] |
| --------- | ------------------- | ------------------- | --------------------- |
| -eq       | Égal a              | [ "$a" -eq "$b" ]   | [[ "$a" -eq "$b" ]]   |
| -ne       | Different de        | [  "$a" -ne "$b" ]  | [[ "$a" -ne "$b" ]]   |
| -lt       | Inférieur a         | [ "$a" -lt "$b" ]   | [[ "$a" -lt "$b" ]]   |
| -le       | Inférieur ou égal a | [ ${a} -le ${b} ]   | [[ ${a} -le ${b} ]]   |
| -gt       | Supérieur a         | [ ${a} -gt ${b} ]   | [[ ${a} -qt ${b} ]]   |
| -ge       | Supérieur ou égal a | [ ${a} -ge ${b} ]   | [[ ${a} -ge ${b} ]]   |

**Comparaison de chaines** 

```bash

= # Egal à
== # Egal a  pas suportté avec [] seuelement avec [[]]
< # inferieur lexicographiquement => ECHAPPEMENT dans [] \<
> # superieur lexicographiquement => ECHAPPEMENT dans [] \>

```

**Tests sur les chaines** 

```shell
-z Vrai si la chaine est vide [ -z "$str" ]  [[ -z "$str" ]]
-n Vrai si la chaine n'est pas vide [ -n "$str" ] [[ -n "$str" ]]
```

**Tests sur les fichiers** 

```bash
-e Vrai sir le fichier existe [ -e "fichier" ]  [[ -e "fichier" ]]
-f Vrai si le fichier est regulier [ -f "fichier" ] [[ -f "fichier" ]]
-d Vrai si c'est un répertoire [ -d "repertoire" ] [[ -d "repertoire" ]]
-L Vrai si c'est un lien symb [ -L "lien" ] [[ -L "lien"]]
-r Vrai si le fichier est lisible [ -r "fichier" ] [[ -r "fichier" ]]
-w Vrai si le fichier est modifiable [ -w "fichier" ] [[ -w "fichier" ]]
-x Vrai si le fichier est exécutable [ -w "fichier" ] [[ -w "fichier" ]]
```

**Opérateurs logiques** 

```bash
-a ET logique [ "$a" -eq 1 -a "$b" -eq 2 ] sur [[]] non recommandé
-o OU logique [ "$a" -eq 1 -o "$b" -eq 2 ] sur [[]] non recomandé
&& ET logique [] non supporté             [[ "$a" -eq 1 && "$b" -eq 2 ]]
```

### Test avancés avec [[]] uniquement

**Expressions régulières ( regex )**

```bash
str="abcd123"
if [[ $str =~ ^[a-z]+[0-9]+$ ]];then
	echo "La chaine correspond au pattern"
fi
```
## Opérateurs  avec ()

Evaluer des expressions arithmétiques 

0 si expression est vrai
1 si expression est fausse

```bash
a=5
b=10

((sum = a + b ))
echo "somme: $sum"

(( a++ ))
echo "Valeur de a après incrémentation : $a" 
```



# Fonctions

**Déclaration de la fonction**

```bash
ma_fonction(){
	echo "Ceci est une fonction simple"
}
```

**Appel de la fonction** 

```bash
ma_fonction
```

**Fonctions avec paramètres** ( pas la même portée que les arguments du script )

```bash
ma_fonction(){
	echo "Premier paramètre : $1"
	echo "Deuxieme paramètre : $2"
	echo "Troisieme paramètre : $#"
}
ma_fonction "Bonjour" "Monde"

Resultat :
Premier paramètre : Bonjour
Deuxieme paramètre : Monde
Troisieme paramètre : 2
```

```bash
#!/bin/bash 
echo "Argument global (script) : $1" # Argument passé au script 

ma_fonction() { 
	echo "Argument dans la fonction : $1" # Argument passé à la fonction }
}	
ma_fonction "Bonjour"

./script.sh global_arg

SORTIE:

Argument global (script) : global_arg
Argument dans la fonction : Bonjour

```


Si fonction n'a pas de paramètres elle hérite des arguments globaux 

**Fonction complete** 


```bash
#!/bin/bash

# Déclaration d'une variable globale
global_var="Je suis globale"

ma_fonction_complexe() {
    # Déclaration de variables locales à partir des arguments de la fonction
    local arg1=$1
    local arg2=$2
    local -n tableau=$3  # Utilisation de 'nameref' pour manipuler un tableau global passé par référence
    local assoc_key=$4   # Argument pour accéder à une clé du tableau associatif
    local result

    echo "=== Fonction Complexe ==="
    echo "Argument 1 (local) : $arg1"
    echo "Argument 2 (local) : $arg2"

    # Afficher les éléments du tableau passé
    echo "Éléments du tableau passé :"
    for element in "${tableau[@]}"; do
        echo " - $element"
    done

    # Utilisation d'un tableau associatif local
    declare -A assoc_array
    assoc_array["clé1"]="valeur1"
    assoc_array["clé2"]="valeur2"

    echo "Valeur associée à $assoc_key : ${assoc_array[$assoc_key]}"

    # Modifier la variable globale dans la fonction
    global_var="Modifiée dans la fonction"

    # Subshell pour un répertoire temporaire
    (
        cd /tmp
        echo "Dans un subshell : $(pwd)"
    )
    echo "Après le subshell : $(pwd)"

    # Utilisation d'une boucle while
    echo "Boucle while : Affichage de 3 nombres"
    local i=1
    while (( i <= 3 )); do
        echo "Nombre : $i"
        ((i++))
    done

    # Utilisation de conditions if/elif/else
    if [ "$arg1" -eq "$arg2" ]; then
        echo "Les deux arguments sont égaux"
    elif [ "$arg1" -gt "$arg2" ]; then
        echo "Le premier argument est plus grand que le deuxième"
    else
        echo "Le premier argument est plus petit que le deuxième"
    fi

    # Structure case
    case $arg1 in
        1) echo "Argument 1 est 1" ;;
        2) echo "Argument 1 est 2" ;;
        *) echo "Argument 1 est autre chose" ;;
    esac

    # Retourner une valeur complexe (via echo ou variable globale)
    result="Résultat calculé dans la fonction"
    echo "$result"
}

# Appel de la fonction avec différents paramètres
tableau_exemple=("valeur1" "valeur2" "valeur3")

# Appel de la fonction et récupération des valeurs
fonction_result=$(ma_fonction_complexe 1 3 tableau_exemple "clé1")
echo "Retour de la fonction : $fonction_result"

# Vérifier la modification de la variable globale
echo "Variable globale après fonction : $global_var"
```