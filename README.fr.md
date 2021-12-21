# os_filesystem-un système de fichiers virtuel (C++)

## introduction

Il s'agit d'un système de fichiers virtuel de type Linux. Le système est porté par un fichier de disque virtuel, qui simule la lecture et l'écriture du disque par la lecture et l'écriture de fichiers, et n'implique pas de pilotes sous-jacents.

Pour écrire un système de fichiers simple de type Linux, vous devez d'abord concevoir un cadre de base qui inclut l'inode, le bloc, le superbloc, la disposition du disque virtuel, l'allocation d'espace et d'autres informations. Le début du système de fichiers est un superbloc, qui contient des informations importantes sur le système, notamment le nombre et la taille des inodes et des blocs. Pour l'inode, d'une manière générale, il doit occuper un pour cent de l'espace disque, mais il s'agit d'un petit système avec une taille totale d'un peu plus de 5M, donc l'espace alloué à la zone inode est très petit, et la plupart des l'espace restant est la zone de bloc.

Le plan global du système de fichiers est le suivant :  
![](./screenshots/00.png)

由于写程序的时候时间比较紧张，只写了4天就去验收，所以代码没来得及优化，有的地方会显得冗余，大家不要见怪。

Bien que le temps soit limité, il implémente également la fonction d'un éditeur vi. L'écriture est relativement simple et le code est brouillon. Il est temps de l'améliorer.

En général, le code n'a pas encore été optimisé, et plus de commentaires sont les bienvenus, et plus de défauts sont relevés.

## comment utiliser

### étape 1 : Télécharger le projet

`git clone https://github.com/windcode/os_filesystem.git`

### Étape 2 : Ouvrez le projet avec VC++6.0

Double-cliquez dans le catalogue**MingOS.dsw**Fichier, ou faites glisser le fichier vers l'interface VC++6.0.

### étape 3 : compiler, lier, exécuter

![](./screenshots/0.png)

### ou

### étape 1 : exécuter directement**/Déboguer**Sous dossier**MingOS.exe**document

## caractéristique

-   Première exécution, créez un fichier de disque virtuel

![](./screenshots/1.png)

-   système de connexion

L'utilisateur par défaut est root et le mot de passe est root

![](./screenshots/2.gif)

-   Commande d'aide (aide)

![](./screenshots/3.gif)

-   Ajout d'utilisateur, suppression, connexion, déconnexion (useradd, userdel, logout)

![](./screenshots/5.gif)

-   Modifier les autorisations de fichier ou de répertoire (chmod)

![](./screenshots/6.gif)

-   L'écriture et la lecture sont restreintes par des autorisations

![](./screenshots/7.gif)

-   Ajout et suppression de fichiers/dossiers (touch, rm, mkdir, rmdir)

![](./screenshots/8.gif)

-   Afficher les informations système (super, inode, bloc)

![](./screenshots/9.gif)

-   Imiter un éditeur de texte vi (vi)

![](./screenshots/4.gif)

-   Indexer les informations sur le fichier et le répertoire de gestion des inodes du nœud

-   utiliser**Méthode de lien de groupe**Gérer l'allocation des blocs gratuits
        * **block分配过程：**
    Lorsqu'un bloc doit être alloué, le sommet de la pile de blocs libres prend une adresse de bloc libre en tant que bloc nouvellement alloué.
    Lorsque la pile est vide, la pile dans le bloc libre représenté par l'adresse inférieure de la pile est utilisée comme nouvelle pile de blocs libres.
        * **block回收过程：**
    Lors de la récupération d'un bloc, vérifiez si la pile est pleine. Si elle n'est pas pleine, le pointeur de la pile actuelle est déplacé vers le haut et l'adresse du bloc à récupérer est placée en haut de la nouvelle pile.
    Si la pile est pleine, le bloc à recycler est utilisé comme nouvelle pile de blocs libres, et l'adresse de l'élément inférieur de la pile de blocs libres est définie sur la pile de blocs libres en ce moment.
        * 分配和回收的同时需要更新block位图，以及超级块。

-   Allocation/récupération d'inode
    -   L'allocation et la récupération des inodes sont relativement simples, en utilisant l'allocation et la récupération séquentielles.
    -   Lorsqu'il doit être alloué, recherchez un inode libre de manière séquentielle à partir du bitmap d'inode et renvoyez le numéro de l'inode si la recherche réussit.
    -   Lors du recyclage, mettez simplement à jour le bitmap de l'inode.
    -   L'allocation et la récupération doivent mettre à jour le bitmap d'inode.

## Remarquer

-   L'environnement d'exploitation est VC++6.0
