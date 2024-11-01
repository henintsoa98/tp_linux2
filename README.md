# I - (Premiers pas)
## 1 - Recopier le script suivant dans un éditeur de texte (script.bash) et l’exécuter.
```bash
#!/bin/bash
for i in `seq 1 10` ### for i in $(seq 1 10)
do
echo "Rock n roll"
done
```
```bash
bash script.bash
```
## 2 - Que fait le script suivant ? Décrire ligne par ligne.
```bash
#!/bin/bash
i1=`ls -a`
i2="grep -R `tutu` ∼"
echo $i1
echo $i2
```
Le script affiche le contenu d'un commande et le contenu d'une chaine de caractere \
L1 : Utiliser l' interpreteur bash \
L2 : La chaine entre \`\` est exécutée, le resultat est stocké dans i1 \
L3 : La chaine entre "" reste une chaine de caractere tandis que la chaine entre \`\` sera executé mais sera partie du chaine de caractere, et le resutat est stocké dans i2 \
L4 : affiche le contenu de i1 \
L5 : affiche le contenu de i2 \
## 3 - Ecrire un script qui prend en entrée plusieurs arguments et qui va afficher :
— son nom \
— son nombre d’arguments \
— le numéro du processus courant \
— la valeur de chaque argument \
(script2.bash)
```bash
#!/bin/bash

echo " Nom du script      : $0"
echo "Nombre d'argument   : $#"
echo "Numero de processus : $$"
echo "Valeur de chaque argument"
for i in `seq 1 1 $#`
do
 eval echo "argument $i : \$${i}"
done
```
```
bash script2.bash
```
# II - (Propriétés et comptage des fichiers)
## 1 - Créer un dossier shell et créer plusieurs fichiers à l’intérieur (ils peuvent être vides) :
— p1.c \
— p2.pas \
— p3.py \
— memo \
— p4.c \
— p5.txt \
— p6.sh \
— p7.sh \
```bash
mkdir shell
```
```bash
echo "int main () {return 0;}" >> shell/p1.c
```
```bash
touch shell/p2.pas
```
```bash
touch shell/p3.py shell/memo
```
```bash
cd shell
```
```bash
touch p4.c
```
```bash
touch p5.txt p6.sh p7.sh
```
## 2 - Mettre les droits en exécution aux fichiers p3.py, p6.sh et p7.sh
```bash
chmod +x p3.py p6.sh p7.sh
ls --color
```
## 3 - Dans le répertoire parent de shell, créer un script find_exec.sh qui doit être appelé avec un seul argument qui doit obligatoirement être un répertoire.
```bash
cd ..
```
(find_exec.sh)
```bash
#!/bin/bash

if [[ "$#" == "1" ]]
then
 if [[ -d "$1" ]]
 then
  echo "$1 is a directory"
 else
  echo "$1 is not a directory"
 fi
else
 echo "Usage : $0 [directory]"
fi
```
```bash
bash find_exec.sh
```
```bash
bash find_exec.sh invalidDir
```
```bash
bash find_exec.sh shell
```
## 4 - Modifier le script pour qu’il liste tous les fichiers, et affiche ceux qui sont exécutables.
(find_exec.sh)
```bash
#!/bin/bash

if [[ "$#" == "1" ]]
then
 if [[ -d "$1" ]]
 then
  echo -e "$1 is a directory\nList of executable :"
  find $1 -type f -executable
 else
  echo "$1 is not a directory"
 fi
else
 echo "Usage : $0 [directory]"
fi

```
## 5 - modifier le script pour qu’il affiche maintenant la taille des fichiers exécutables.
```bash
#!/bin/bash

if [[ "$#" == "1" ]]
then
 if [[ -d "$1" ]]
 then
  echo -e "$1 is a directory\nList of executable :"
  find $1 -type f -executable -exec ls -lh {} +
  # or
  echo "Plus structurer"
  find $1 -type f -executable -exec ls -lh {} + | awk '{print $9" : "$5}'
 else
  echo "$1 is not a directory"
 fi
else
 echo "Usage : $0 [directory]"
fi

```
# III - (Traitement des notes)
## 1 - Créer le fichier notes.txt suivant :
```
Liste des notes CY-TECH : préING2 (2022-2023)
Nom étudiant:note
Paco:10
Pépé:5
José:18
Hermano:15
Pablo:19.5
Luis:2
Toto:20
Titi:7
```
## 2 - Créer un script qui permet d’afficher le contenu de ce fichier mais avec les lignes triées par notes décroissantes. Les 2 premières lignes doivent être affichées en premier : seules les lignes avec notes doivent être triées. Le script prendra en paramètre le fichier notes.txt, vérifiera que le fichier existe et que c’est bien un fichier, vérifiera que le nombres de lignes à l’intérieur du fichier est compatible avec le traitement demandé, et enfin affichera le contenu.
(script3.bash)
```bash
#!/bin/bash

if [[ "$#" == "1" ]]
then
 if [[ -f "$1" ]]
 then
  echo -e "$1 is a file"
 else
  echo "$1 is not a file"
  exit
 fi
else
 echo "Usage : $0 [files]"
 exit
fi

# Headers
head -n 2 notes.txt

echo "--------------------------------------------------------"

# Notes
NOTES=$(cat notes.txt |sed "1d;2d")

# Verification si le fichier contient des notes
if [[ "$NOTES" ]]
then
 echo "Notes :"
 echo "$NOTES"
else
 echo "Pas de notes"
 exit
fi

# Triages
echo "Notes decroissante :"
echo "$NOTES" | sort -t : -k 2 -n -r
# -t : separator, -k : Position, -n : tri numerique, -r : decroissante
```
## 3 - Modifier le script pour afficher un message personnalisé si une erreur survient. Ce message devra être envoyé sur l’erreur standard et non sur la sortie standard. Le script retournera le code 0 si tout s’est bien passé.
```bash
#!/bin/bash

if [[ "$#" == "1" ]]
then
 if [[ -f "$1" ]]
 then
  echo -e "$1 is a file"
 else
  echo "$1 is not a file" >&2
  exit
 fi
else
 echo "Usage : $0 [files]" >&2
 exit
fi

# Headers
head -n 2 notes.txt

echo "--------------------------------------------------------"

# Notes
NOTES=$(cat notes.txt |sed "1d;2d")

# Verification si le fichier contient des notes
if [[ "$NOTES" ]]
then
 echo "Notes :"
 echo "$NOTES"
else
 echo "Pas de notes" >&2
 exit
fi

# Triages
echo "Notes decroissante :"
echo "$NOTES" | sort -t : -k 2 -n -r
# -t : separator, -k : Position, -n : tri numerique, -r : decroissante
exit 0
```
Affiche les erreurs dans un fichier
```bash
bash script3.bash notes.tx 2> erreur.txt
cat erreur.txt
```
# IV - (Traitements des fichiers)
## 1 - Créer un script qui va afficher le contenu détaillé du dossier courant avec les inodes et les fichiers cachés, mais sans la première ligne qui indique le total du dossier
script4.bash
```bash
#!/bin/bash

ls -lai | sed "1d"
```
```bash
bash script4.bash
```
## 2 - Modifier le script pour qu’il stocke le résultat de la commande précédente dans une variable et qu’il affiche chaque ligne de cette variable. Que constatez vous sur l’affichage ? Modifier le script pour qu’il affiche correctement chaque ligne.
```bash
#!/bin/bash
LS=$(ls -lai | sed "1d")

# Premier essai
echo $LS

# rectification
echo "$LS"
```
```bash
bash script4.bash
```
## 3 - Pour chaque ligne affichée, modifier le script pour filtrer et ne garder que l’inode, les droits, la taille en octets et le nom du fichier.
```bash
#!/bin/bash
LS=$(ls -lai | sed "1d")

echo "$LS" | awk '{print $1" "$2" "$6" "$10}'
```
```bash
bash script4.bash
bash script4.bash | column -t
```
## 4 - Stocker dans des variables séparées l’inode, le type de fichier (on peut utiliser les droits affichés précédemment), la taille et le nom.
```bash
#!/bin/bash
LS=$(ls -lai | sed "1d")

DATA=$(echo "$LS" | awk '{print $1" "$2" "$6" "$10}')

INODE=$(echo "$DATA" | awk '{print $1}')
TYPE=$(echo "$DATA" | awk '{print $2}' | sed "s/d/d /;s/-/f /" | awk '{print $1}')
TAILLE=$(echo "$DATA" | awk '{print $3}')
NOM=$(echo "$DATA" | awk '{print $4}')
echo "inode  : "$INODE
echo "type   : "$TYPE
echo "taille : "$TAILLE
echo "nom    : "$NOM
```
```bash
bash script4.bash
```
## 5 - Afficher avec votre propre formalisme le nom du fichier et son type , ainsi que sa taille et son inode.
```bash
#!/bin/bash
LS=$(ls -lai | sed "1d")

DATA=$(echo "$LS" | awk '{print $1" "$2" "$6" "$10}')

INODE=$(echo "$DATA" | awk '{print $1}')
TYPE=$(echo "$DATA" | awk '{print $2}' | sed "s/d/d /;s/-/f /" | awk '{print $1}')
TAILLE=$(echo "$DATA" | awk '{print $3}')
NOM=$(echo "$DATA" | awk '{print $4}')

echo -e "NOM "$NOM"\nTYPE "$TYPE"\nTAILLE "$TAILLE"\nINODE "$INODE | column -t
echo "Zoom out to see a table"

```
```bash
bash script4.bash
```
# V - (Comparaison de fichiers)
— Créer un script qui prendra obligatoirement 2 fichiers en arguments et qui devra comparer que le contenu est le même. \
Ce script devra vérifier les paramètres d’entrée (nombre et type) avant de faire le traitement \
— Le script va calculer une signature sur le contenu des fichiers à l’aide d’une commande de hachage de votre choix. Il va ensuite comparer les valeurs de hash pour indiquer le résultat (affichage d’un message d’erreur vers l’erreur standard si les fichiers sont différents, sinon aucun affichage. Le script renverra également un code d’erreur (0 ou 1) pour indiquer le résultat de la comparaison.
```bash
#!/bin/bash

# verification arguments
## nombre
if [[ $# -ne 2 ]]
then
 echo "Usage : $0 [file1] [file2]"
 exit
fi
## type
if [[ ! -f "$1" || ! -f "$2" ]]
then
 echo "One or two file are invalid"
 exit
fi

# hash
HASH1=$(sha256sum $1 | awk '{print $1}')
HASH2=$(sha256sum $2 | awk '{print $1}')

# comparaison
if [[ "$HASH1" == "$HASH2" ]]
then
 exit 0
else
 echo "les fichiers sont différents" >&2
 exit 1
fi
```
```
bash script5.bash
```
```
echo "fichier1" > f1
echo "fichier2" > f2
echo "fichier1" > autre
```
```
bash script5.bash f1 f2
```
```
bash script5.bash f1 autre
```
— Existe-t-il un moyen plus simple que d’écrire un script pour comparer le contenu de fichiers ?
```bash
diff f1 f2
```
```
diff f1 autre
```
