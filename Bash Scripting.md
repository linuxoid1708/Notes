
# Special variables




## Variables liées aux arguments

```bash
$#       => nombres argumen
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