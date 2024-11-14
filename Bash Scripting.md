
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
