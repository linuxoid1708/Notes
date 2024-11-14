
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

```