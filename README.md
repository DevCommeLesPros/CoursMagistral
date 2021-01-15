1. toc
{:toc}

# C’est important l’environnement

## L’environnement de développement

<figure style="float: right">
    <img src="/assets/images/without-IDE.svg"/>
    <figcaption>Faites connaissance avec l'«acteur».</figcaption>
</figure>
Développer un programme fait souvent appel à utiliser bien d’autres outils et programmes.
L’éditeur de texte et le compilateur sont deux exemples.
On modifie notre code dans l’éditeur, ensuite on va à l’invite de commandes, on lance le compilateur (comme `gcc`) pour compiler notre code en un programme et on lance le programme pour voir ce que ça donne.
Habituellement, le programme ne fonctionne pas du premier coup.
Il y a un bogue.
Soit on retourne à l’éditeur pour corriger notre code en essayant de comprendre ce qui ne va pas, soit on utilise un débogueur (comme `gdb`) pour s’aider.
Et on recompile.
Et on re-débogue.
Et on recommence.
Et on recommence...
Si seulement il y a avait mieux...

Après avoir répété le même procédure manuelle trois fois, «[le bon développeur](https://www.google.com/search?q=les+inconnus+les+chasseurs)» se dit : «Il n’y en aura pas de quatrième !»
Et l’environnement de développement fût.

Ce qu’on appelle «[environnement de développement intégré](https://fr.wikipedia.org/wiki/Environnement_de_d%C3%A9veloppement)» ou «EDI» est généralement un programme qui regroupe et met à portée de main tous les outils nécessaires pour mener à bien la tâche du·de la développeur·se.
Le plus souvent, ça ressemble à un éditeur de texte avec plus de fonctionnalités.
Outre compiler (ou interpréter) du code et le déboguer facilement, un EDI peut aussi automatiser d’autres étapes de développement de logiciel comme :

- synchroniser le code commun entre coéquipiers.
- mettre à jour des bibliothèques logicielles de tierces parties.
- assembler des fichiers en une archive finale.
- déployer une application web sur un ou plusieurs serveur.

<figure style="float: right;">
    <img src="/assets/images/with-IDE.svg"/>
</figure>
Il existe de nombreux EDIs.
Certains sont axés sur la plateforme de développement (`AndroidStudio` pour Android, `Xcode` pour la «iFamille», etc.), d’autres sur le langage de programmation (`IntelliJ IDEA` pour Java, `IDLE` pour Python), d’autres encore restent plus agnostiques et permettent à l’utilisateur de les configurer pour la tâche en question, quels que soient la plateforme ou le langage.
Pour ce cours, nous allons utiliser [Visual Studio Code](https://code.visualstudio.com/) qui fait parti de la troisième catégorie.

## Est-ce bien nécessaire ?

La nécessité d’utiliser un EDI est souvent dictée par l’échelle du projet.
À petit projet, une petite poignée d’outils mais quand viendra le temps de travailler sur un projet de bonne taille -avec des centaines de fichiers de code, de test, de script, de configuration, etc.- un EDI est des plus bienvenu.

## D’autre questions ?

**Q :** J’utiliserai `Visual Studio Code` dans le futur ?  
**R :** Les chances sont pour que vous utilisiez une application semblable.

**Q :** On devrait toujours commencer à programmer avec un EDI, non ?  
**R :** Comme en cuisine ou dans d’autres domaines, apprendre à faire les choses un peu plus «manuellement» un première fois n’est jamais perdu.
Une fois qu’on a compris «ce qui se passe sous le capot», on peut s’atteler plus confortablement à de plus grand projets.

*[démo]*

> Nous avons vu `Xcode`, un EDI sous macOS qui intègre un compilateur, un débogueur et bien d’autres outils. Cet EDI, créé par Apple est l’EDI à utiliser pour développer des applications pour iOS, iWatch, macOS, etc. avec, comme langage de programmation C, C++, Objective-C ou Swift.
On peut aussi utiliser Xcode pour des applications à utiliser à l’invite de commandes.

> Nous avons aussi vu `Visual Studio Code`, un EDI disponible sous Linux, macOS et Windows.
Cet EDI se veux extrêmement flexible et peut être configurer pour développer des programmes en divers langages.
Le fichier `.vscode/tasks.json` sert à configurer comment compiler notre programme.
Le fichier `.vscode/launch.json` sert à configurer comment lancer (ou déboguer) notre programme.

# On fait tous des erreurs

## Déboguer

Avez-vous déjà écrit une dissertation sans jamais vous relire ?
Sans jamais faire ne fût-ce qu’une seule coquille ?
Probablement pas.
Même les auteurs de romans les plus chevronnés se relisent pour corriger des erreurs de grammaire ou remanier la structure de leur histoire.

Développer un programme c’est écrire une histoire avec des formules mathématiques et des énoncés logiques.
Comme un roman, c’est un travail de création et il n’y a pas de perfection.
Seulement, les éléments mathématiques et logiques utilisés, eux, doivent être sans erreurs.

Que faire quand le programme ne se comporte pas comme prévu ?
Quand pour un `X` donné, on obtient pas le `Y` désiré ?
Il y a plusieurs façon d’analyser la source du problème.

## Déboguer par inspection visuelle

Pour des programmes simples de quelques lignes, relire le code en gardant en tête un modèle de ce à quoi le code *devrait* accomplir peut suffire.
Seulement...
Faisons un exercice et pensez à une bicyclette.
Pourriez-vous la décrire parfaitement ?
Ou peut-être la dessiner ?
L’expérience a déjà [été menée](http://www.gianlucagimini.it/prototypes/velocipedia.html) et voici le résultat de rendus 3D à partir de dessins faits de mémoire :

<div class="row">
  <div class="column">
    <figure>
        <img src="assets/images/velocipedia-by-gianluca-gimini-5.jpg" style="width:100%">
        <figcaption>© Gianluca Gimini</figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
        <img src="assets/images/velocipedia-by-gianluca-gimini-3.jpg" style="width:100%">
        <figcaption>© Gianluca Gimini</figcaption>
    </figure>
  </div>
  <div class="column">
    <figure>
        <img src="assets/images/velocipedia-by-gianluca-gimini-23.jpg" style="width:100%">
        <figcaption>© Gianluca Gimini</figcaption>
    </figure>
  </div>
</div> 

Amusant mais pas très mobile...
Voyons si vous faites aussi bien avec du code.
Imaginez une fonction qui décale toutes les valeurs d’un tableau de façon circulaire.
Maintenant, avec vos yeux seulement, trouvez où est l’erreur (ou les erreurs) dans le programme suivant :

~~~ c
// Donne la taille d'un tableau.
#define taille_de(A) (sizeof(A) / sizeof(A[0]))

// Décale un tableau de façon circulaire. T[0] <- T[1], ..., T[N] <- T[0]
void rotation(int T[])
{
    int const taille = taille_de(T);
    if(taille >= 2)
    {
    int const premiere_valeur = T[0];
    for(int i = 0; i != taille - 1; ++i)
    {
        T[i] = T[i + 1];
    }
    T[taille - 1] = premiere_valeur;
    }
}
~~~

## Déboguer par trace

Quand l’inspection visuelle devient insuffisante, la deuxième arme utilisée est souvent les «traces».
Comme le Petit Poucet qui laisse des traces derrière lui, on affiche à l’écran des informations qui nous apparaissent pertinentes au fur de l’exécution de notre programme.
En C, on utilise la fonction `printf` pour ce faire.

Aidons-nous à résoudre le problème dans le code précédent avec quelques traces:

~~~ c
// Donne la taille d'un tableau.
#define taille_de(A) (sizeof(A) / sizeof(A[0]))

// Décale les valeurs d'un tableau de façon circulaire.
void rotation(int T[])
{
    int const taille = taille_de(T);
    printf("taille_de(T) = %d\n", taille);
    if(taille >= 2)
    {
    int const premiere_valeur = T[0];
    for(int i = 0; i != taille - 1; ++i)
    {
        printf("T[%d] = %d\n", i, T[i + 1]);
        T[i] = T[i + 1];
    }
    printf("T[%d] = %d\n", taille - 1, premiere_valeur);
    T[taille - 1] = premiere_valeur;
    }
}

int chiffres[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
rotation(chiffres);
~~~

Et voici ce qu’on observe:

~~~ c
taille_de(T) = 2
T[0] = 2
T[1] = 1
~~~

Hmm...

## Déboguer avec un débogueur

Dans le cas d’un programme qui, dans son fonctionnement normal, affiche aussi des informations à l’écran, ces traces peuvent devenir indésirables et le·la développeur·se doit soit les désactiver, soit les enlever entièrement.
Ce travail d’écrire et d’enlever des traces peut devenir rapidement fastidieux et il devient pratiquement impossible pour des programmes dépassant les milliers de lignes et où le nombre de paramètres dépassent nos facultés mentales.

Pou tout travail de débogage d’un programme substantiel et où le·la développeur·se a le loisir d’avoir le code à sa disposition, l’utilisation d’un [débogueur](https://fr.wikipedia.org/wiki/D%C3%A9bogueur) est recommandée.
Un débogueur permet d’exécuter un programme bien lentement, ligne-par-ligne, et d’observer la valeur de toutes les variables à tout instants.
On peut aussi exécuter le programme jusqu’à un certain point ou une certaine condition de notre choix.

Voici ce qu’on observe avec `Visual Studio Code` si on fige l’exécution de notre programme problématique juste après être entré dans la fonction `rotation` :

<figure>
    <img src="/assets/images/vscode-debug.png"/>
    <figcaption>Voyons voir l’onglet «VARIABLES».</figcaption>
</figure>
<figure>
    <img src="/assets/images/vscode-debug-variable.png"/>
    <figcaption>Et ça nous donne...</figcaption>
</figure>

Tiens donc, la variable `T` serait de type pointeur...
N’était-ce pas plutôt un tableau de taille 10 ?
Qu’est-ce que ça signifie ?
C’est le dernier indice dont nous avions besoin.
(Attention, au moment où l’image ci-haute a été capturée, la variable `taille` n’avait pas encore été déterminée.
La valeur `32766` est une valeur au hasard.
Ceci dit, comme le type de la variable `T` est un pointeur, la macro `taille_de` déterminera que la taille est de `2` ce qui n’est toujours pas la valeur attendue.)

## Déboguer par journal

Il peut être difficile voire impossible de déboguer un programme avec les méthodes mentionnées ci-haut.
On devient alors détective et on analyse ~~la scène du crime~~ le bogue à postériori avec les seules informations disponibles.
Habituellement, un journal consiste en une sauvegarde de traces exhaustives laissées par le programme.
Ces traces ne sont pas affichées sur un écran pour que quelqu’un les visionnent en temps réel mais plutôt écrites dans un ficher (`.log`) ou une base de données.

Cas où des journaux sont utiles:

- programmes qui ne peuvent être observés directement (p. ex. contrôle d’un réfrigérateur).
- programmes qui sont exécutés sur une très longue durée dans le temps (p. ex. contrôle d’une centrale électrique).
- programmes avec plusieurs fils d’exécution (p. ex. serveur multi-clients).
- programmes avec une exécution en temps réel (p. ex. flux multimédia).
- programmes distribués (p. ex. [LHC@Home](https://lhcathome.cern.ch/lhcathome/))

Dans ces circonstances, les journaux servent de «[boîte noire](https://fr.wikipedia.org/wiki/Bo%C3%AEte_noire_(a%C3%A9ronautique))» qu’on analyse pour comprendre une erreur survenue antérieurement et réparer la prochaine version de notre programme qui sera distribué aux utilisateurs.

*[démo]*

> Nous avons vu que `Visual Studio Code` est très utile pour nous aider à déboguer un programme erroné à «la vitesse de l’humain».
Le débogueur nous permet de visualiser l’état de notre programme (les fonctions appelées, les valeurs des variables) à chaque ligne de code exécutée.

> Pour lancer notre programme ou continuer son exécution : `F5`.

> Pour ajouter un point d’arrêt dans l’exécution du programme :  cliquer dans la marge gauche ou `F9`.

> Pour avancer de une ligne de code : `F10`.

> Pour entrer dans une fonction qui est sur le point d’être appelée : `F11`.

> Nous avons vu un exemple de code qui sert à garder de l’information utile dans un journal :
~~~ c
char time_s[100];
time_t now = time(0);
strftime(time_s, 100, "%Y-%m-%d %H:%M:%S.000", localtime(&now));
fprintf(journal, "[%s] %s appelée avec '%s' comme paramètre.\n", time_s, __PRETTY_FUNCTION__, chaine);
~~~

> Ce code, inclus dans la fonction `inverse`,  nous dit quand et comment la fonction a été appelée :
`[2020-02-07 17:03:18.000] char *inverse(char *) appelée avec 'banane' comme argument.`

# Comment se protéger de soi-même

## Gestion de version I

On écrit du code, on écrit, on écrit...
Puis viens un moment ou on essaie quelque chose mais ça ne marche pas.
Alors on essaie autre chose et puis ça marche un peu.
On voudrait essayer une troisième option mais en gardant un peu de la deuxième option alors on commence à commenter ce qu’on vient d’écrire pour la garder à portée de main.
Et on continue.
Le tout agrémenté de plusieurs `Undo`/`Redo` qui nous ramène un peu en arrière.
Mais...
Ah !
On veut revenir en arrière sans perdre ce petit bout de code qu’on vient d’écrire alors on le copie avec `Ctrl-C` en faisant bien attention de ne pas faire un autre `Ctrl-C` car on perdrait le premier bout de code d’avant...
Aller, c’est bon.
`Ctrl-V`.
Quoi ?
Mais c’est pas ça que je voulais.
Vite !
`Undo`, `undo`, `undo`...
Ah, non, c’est dans le mauvais sens.
Vite !
`Redo`, `redo`, `redo`...
Ah, non, mais là j’ai perdu mon historique de `undo` !

Lorsqu’on travaille sur un projet qui dure dans le temps et/ou sur lequel travaillent plus d’une personne, un des outils essentiels à utiliser est un programme de [gestion de version](https://fr.wikipedia.org/wiki/Gestion_de_versions).
Parmi ces programmes, le plus populaires en 2020 est [git](https://fr.wikipedia.org/wiki/Git).

On les appelle des programmes de gestion de «version» parce qu’on les utilise pour créer un historique de notre code.
Voyez ces versions comme tout autant de points de restauration vers lesquels vous pouvez aller et revenir comme vous voulez.

Composons une ébauche de lettre :

> Bonjour Michèlle. Comment va-tu ?

***
> Bonjour Michèle. Comment va-tu ?

***
> Bonjour Michèle. Comment vas-tu ?

***
> Bonjour Michèle. Comment allez-vous ?

Nous nous sommes corrigé quelque fois en cours de route et nous avons même décider de changer de formule.

`git` fonctionne par deltas, c’est-à-dire par une liste de modifications pour aller de «avant» à «après».
Visualisons cette évolution comme suit :

~~~
A -> B -> C -> D
~~~

| Message | Étape | git |
| - |:-:| - |
| Bonjour Michèlle. Comment va-tu ?     | A | Bonjour Michèlle. Comment va-tu ? |
| Bonjour Michèle. Comment va-tu ?      | B | `-Michèlle +Michèle`              |
| Bonjour Michèle. Comment vas-tu ?     | C | `-va +vas`                        |
| Bonjour Michèle. Comment allez-vous ? | D | `-vas-tu +allez-vous`             |

L’addition de tous ces deltas, dans l’ordre, au texte de départ nous amène à la dernière «version».
`git` conserve tous ces deltas dans une base de données.
C’est-à-dire que `git` conserve la toute première ébauche de notre texte dans son intégralité ainsi que tous les deltas que nous avons apportés au fil du temps.
(En fait, je simplifie beaucoup mais avec cette image en tête, vous devriez pouvoir vous débrouiller.)

Comment ces deltas ou points de restauration sont-ils créés ?
Avec `Ctrl-S` ?
Ou est-ce automatique ?
Ni l’un ni l’autre.
Il faut les créer soi-même.
À quel moment ?
Ce n’est pas une science exacte.
Il faut seulement comprendre que ces points de restauration ne coûtent rien et qu’il est préférable de les créer à des moments où le code «se tient».

Très bien. Nous avons maintenant des points de restauration, nous n’avons plus peur de perdre nos idées.

*[démo]*

> Nous savons comment utiliser `git` pour créer un dépôt qui contiendra l’historique de notre projet.

> Le dépôt est premièrement initialisé avec `git init`.

> Si le dépôt existe déjà (sur github.com par exemple), nous en faison une copie locale avec `git clone`.

> Ensuite, nous ajoutons nos modifications à la liste des modifications à enregister avec `git add [nom-du-fichier]`.

> Les modifications seront enregistrées dans une nouvelle «version» en utilisant la command `git commit -m "Explication des modifications."` 

> À tout moment, nous pouvons connaître l’état du dépôt et des modifications courantes avec `git status`.

## Les mondes parallèles

Il est souvent désirable de travailler sur une nouvelle fonctionnalité de façon plus isolée.
Lorsqu’on travaille en équipe, c’est même impératif.
Pour ce faire, on travaille sur un chemin parallèle : une [branche](https://fr.wikipedia.org/wiki/Branche_(gestion_de_configuration)).
Revenons à notre message pour Michèle.
Nous sommes un peu à cours d’argent :

> T’as pas vingt balles ?

***
> T’as pas cent balles ?

***
> Vous n’auriez pas cent euros ?

Comme nous n’étions pas sûr de garder cette demande dans notre message final, nous avons créé une branche pour nous y consacrer.
On peut la visualiser comme suit :

~~~
A -> B -> C -> D
          \
           -> E -> F -> G
~~~

Apparemment, cette branche a été crée avant que nous ne devenions plus poli.
L’historique de ces changements ressemble peut-être à ceci :

|  Message | Étape | git |
| - |:-:| - |
| Bonjour Michèlle. Comment va-tu ?                                    | A | Bonjour Michèlle. Comment va-tu ? |
| Bonjour Michèle. Comment va-tu ?                                     | B | `-Michèlle +Michèle` |
| Bonjour Michèle. Comment vas-tu ?                                    | C | `-va +vas` |
| Bonjour Michèle. Comment vas-tu ? T’as pas vingt balles ?            | E | `+T'as pas vingt balles ?` |
| Bonjour Michèle. Comment vas-tu ? T’as pas cent balles ?             | F | `-vingt +cent` |
| Bonjour Michèle. Comment allez-vous ?                                | D | `-vas-tu +allez-vous` |
| Bonjour Michèle. Comment vas-tu ? <br>Vous n’auriez pas cent euros ? | G | `-T'as pas cent balles ? +Vous n'auriez pas cent euros ?` |

J’ai dit «peut-être» parce que dans cette simplification, il n’y a pas de date ni d’heures associés au changement qui nous indiquerais l’exacte évolution des deux branches.
Pour cet exemple, ce n’est pas important.
Ce qui est important est de comprendre qu’avant de se décider si cette demande monétaire fera partie de notre message final, nous y travaillons séparément du reste du message.
Et qu’en même temps, nous avons aussi pu séparément mettre à jour notre introduction.
Nos idées sont restées cloisonnées.

Nous avons maintenant deux choix.
Premièrement, oublier cette demande d’argent qui serait mal vue.
Nous effaçons alors simplement notre branche et nous nous retrouvons avec ce qui reste :

~~~
A -> B -> C -> D
~~~

Deuxièmement, nous pouvons rester audacieux et intégrer la question à notre message :

~~~
A -> B -> C -> D - - - - - -> H
           \                /
            -> E -> F -> G
~~~

Notre historique devient alors :

| Message | Étape | git |
| - |:-:| - |
| Bonjour Michèlle. Comment va-tu ? | A | Bonjour Michèlle. Comment va-tu ? |
| (Comme précédemment.) | (B&nbsp;à&nbsp;G) | (Comme précédemment.) |
| Bonjour Michèle. Comment allez-vous ? Vous n’auriez pas cent euros ? | H | `-vas-tu +allez-vous +Vous n'auriez pas cent euros ?` |

Notez qu’à l’étape `H`, le delta est la somme des deltas entre `C` et `D` et entre `C` et `G`, l’étape `C` étant le point divergence.

Attention : en pratique, au moment de fusionner `D` et `H`, il y aura conflit à cause de la différence entre `vas-tu` d’un côté et `allez-vous` de l’autre.
`git` ne peut pas automatiquement deviner ce qu’il faut garder dans cette situation et c’est à l’ingénieur·e de le spécifier.

*[démo]*

> Nous avons vu que l’historique de notre dépôt peut se composer de plusieurs branches.

> La branche par defaut et sur laquelle nous avons amorcée notre lettre s’appelle `master`.

> Nous avons «reculé dans le temps» pour se retrouver au point `C` de notre historique avec `git checkout [valeur-de-hachage]`.

> Nous avons créé une branche pour travailler en parallèle à la branche `master` avec `git branch [nom-de-la-branche]` et `git checkout [nom-de-la-branche]`.

> Finalement, pour fusionner notre branche temporaire sur la branche `master` nous avons premièrement basculé sur la branche `master` avec `git checkout master` et nous avons fusionné la branche temporaire sur `master` avec `git merge [nom-de-la-branche]`.

> Comme les modifications était relativement importante par rapport au contenu existant, il y a eu «conflit» et nous avons dû résoudre ce conflit manuellement.
Nous avons donc édité le fichier pour le mettre dans l’état désiré et faire `git add [nom-du-fichier]` suivi de `git commit` pour finalisé la fusion.
Les conflits ne surviennent pas toujours.

## Le dépôt

On appelle [dépôt](https://fr.wikipedia.org/wiki/D%C3%A9p%C3%B4t_(informatique)#D%C3%A9p%C3%B4t_de_code_source) la combinaison d’une base de code et de son historique de modifications associés.
Quand on travaille en équipe, il y a un toujours dépôt central qui fait office de point de jonction entre tous les membres de l’équipe et c’est à travers ce dépôt central que les changements apportés par les différents membres sont partagés et synchronisés entre eux.

<figure style="float: right">
    <img src="/assets/images/depot-central.svg"/>
</figure>

Ce dépôt central est sur un serveur qui peut être un serveur privé visible que des membres de l’équipe.
Il peut aussi être un service d’hébergement de code comme http://www.github.com.

Dans le cadre de ce cours, vous allez utiliser GitHub comme dépôt de code pour vos exercices.
Cela rend même possible de pouvoir travailler de deux endroits différents (la maison et l’université) sans avoir à trimbaler une clé USB avec son code dessus.

*[démo]*

> Nous avons vu que GitHub est un service où nous pouvons héberger notre dépôt.

> Nous avons créé un dépôt et synchronisé notre dépôt local avec celui sur GitHub avec `git push`.

## Est-ce vraiment nécessaire ?

Oh, que oui !
Garder l’historique des modifications apportées à une base de code, surtout une base de code qui existe depuis un bon moment et sur laquelle plusieurs personnes ont travaillé, permet de retracer et d’analyser le comment et le pourquoi de son évolution.

En retournant dans le passé on peut aussi analyser à quel moment une erreur aurait été introduite dans le code et plus facilement la corriger.

En travaillant avec des branches on évite de marcher sur les pieds de ses collègues de travail. On peut aussi facilement expérimenter sans bloquer personne.

## D’autres questions ?

**Q :** Il n’y a que `git` qui fait ça ?  
**R :** Non, il y a bien d’autres outils que vous serez peut-être appelé·e à utiliser :

- [Subervsion](https://fr.wikipedia.org/wiki/Apache_Subversion) était, je crois, le plus populaire outil de contrôle de version avant d’être détrôné par `git`.
Il est encore utilisé par des équipes qui travaillerait sur du code moins récent et qui n’ont pas encore migré vers un outil plus moderne (pour, peut-être, de bonnes raisons).

- [Perforce](https://fr.wikipedia.org/wiki/Perforce) est encore apprécié par des équipes qui doivent travailler avec des fichiers binaires (en plus de fichiers textes comme du code) car `Perforce` s’en accommode mieux que `git`.
C’est le cas pour les studios de jeux vidéos qui ont besoin garder l’historique de leurs «[assets](https://fr.wikipedia.org/wiki/Asset#Jeu_vid%C3%A9o)».
L’interface de `Perforce` est aussi plus accessible aux «non-développeur·se·s».

**Q :** Raah ! C’est compliqué `git` !  
**R :** Là, je suis d’accord avec vous.
J’ai souvent trouvé `git` plus compliqué que nécessaire et `git` n’a certainement pas été créé avec la facilité d’utilisation comme critère mais il faut s’y faire.
Il existe plusieurs applications avec une interface graphique pour `git` (y compris des extensions de `Visual Studio Code`) pour se faciliter la vie mais ces applications exposent toute la fonctionnalité de `git` qui peut être étourdissante.

# Un makefile c’est un livre de recette

| **Les Bonnes Recettes de Marino**<br><br>**Sauce Alfredo**<br>**Ingrédients** : beurre, ail, crème, parmesan<br>**Préparation** :<br>1. Dans une poêle, faire revenir l’ail dans du beurre. <br>2. Ajouter la crème. <br>3. Quand la crème s’est épaissit, ajouter le parmesan.<br><br>**Crevettes grillées**<br>**Ingrédients** : beurre, crevettes<br>**Préparation** :<br>1. Nettoyer les crevettes.<br>2. Dans une poêle, faire revenir les crevettes dans du beurre quelques secondes.<br><br>**Fettuccine Alfredo**<br>**Ingrédients** : fettuccine, sauce Alfredo<br>**Préparation** :<br>1. Cuire les pâtes dans un chaudron d’eau bouillante puis égoutter.<br>2. Combiner les pâtes et la sauce Alfredo.<br><br>**Fettuccine Alfredo aux crevettes**<br>**Ingrédients** : fettuccine Alfredo, crevettes grillées<br>**Préparation** :<br>1. Combiner les fettuccine et les crevettes grillées. | **`makefile`**<br><br>sauce-alfredo: beurre ail creme parmesan \| poele<br>&nbsp;&nbsp;faire revenir l’ail dans du beurre dans la poele<br>&nbsp;&nbsp;ajouter la creme<br>&nbsp;&nbsp;attendre que la creme se soit epaissit<br>&nbsp;&nbsp;ajouter le parmesan<br><br>crevettes-grillees: beurre crevettes \| poele<br>&nbsp;&nbsp;nettoyer les crevettes<br>&nbsp;&nbsp;faites revenir les crevettes dans du beurre quelques secondes dans la poele<br><br>fettuccine-alfredo: fettuccine sauce-alfredo \| chaudron<br>&nbsp;&nbsp;faire bouillir de l’eau dans le chaudron<br>&nbsp;&nbsp;cuire les fettuccine<br>&nbsp;&nbsp;égoutter les fettuccine<br>&nbsp;&nbsp;combiner les fettuccine avec la sauce-alfredo<br><br>fettuccine-alfredo-aux-crevettes: fettuccine-alfredo crevettes-grillees<br>&nbsp;&nbsp;combiner les fettucine-alfredo avec les crevettes-grillees |

| **À l’invite de commandes**<br><br>Pour compiler mon code et produire un programme:<br>`$ gcc main.c -o application -lm`<br><br>Pour lancer mon programme:<br>`$ ./application`<br><br>Pour me débarrasser des fichiers générés et repartir à zéro:<br>`$ rm application` | **`makefile`**<br><br>application: main.c<br>&nbsp;&nbsp;gcc main.c -o application -lm<br><br>run: application<br>&nbsp;&nbsp;./application<br><br>clean:<br>&nbsp;&nbsp;rm application<br><br>**À l’invite de commandes**<br>Pour compiler mon code et produire un programme :<br>`$ make application`<br><br>Pour lancer mon programme:<br>`$ make run`<br><br>Pour me débarrasser des fichiers générés et repartir à zéro:<br>`$ make clean` |

## Est-ce bien nécessaire ?

Le programme [make](https://fr.wikipedia.org/wiki/Make) est un programme d’automatisation.
Il peut être utilisé pour faire tout ce qui puisse être décrit comme une liste d’étapes à suivre.
Dans le contexte de la programmation, `make` est utilisé pour **simplifier** et **accélérer** les tâches reliées à la construction des différentes cibles d’une base de code (p. ex. bibliothèques, applications, tests, etc.) ou à son entretien (effacement des fichiers temporaires, installation des programmes, etc.).

## Comment make simplifie notre travail ? 

Considérez qu’une base de code professionnelle peut consister de dizaines, voire des centaines de fichiers, les développeur·se·s serait très rapidement las d’avoir à compiler à répétition tous ces fichiers à l’invite de commandes !
`make` est utilisé pour faire abstraction des commandes à lancer à l’invite de commandes pour compiler ou lier les divers fichiers de la base de code.
Dans l’exemple suivant, la seule invocation de `make test` :

1.  génère un répertoire de sortie `build/Linux`.
1. compile et lie tous les fichiers de code nécessaires à la création de la bibliothèque logicielle `libchiffrage.o`.
1. compile le fichier de code de test en fichier objet `test.o`.
1. lie les fichiers objets `test.o` et `libchiffrage.o`  pour produire l’application `test`.

~~~
$ make test
mkdir -p build
gcc -g -c lib/ROT13.c -I ./lib -o build/ROT13.o
gcc -g -c lib/Cesar.c -I ./lib -o build/Cesar.o
gcc -g -c lib/Vigenere.c -I ./lib -o build/Vigenere.o
ar crs build/libchiffrage.a build/ROT13.o build/Cesar.o build/Vigenere.o
gcc -g -c test/main.c -I ./lib -o build/test.o
gcc build/test.o -Lbuild -lchiffrage -o build/test
~~~

## Comment make accélère notre travail ?

`make` prend en compte la date et l’heure de modification des cibles dépendantes.
Si une cible `X` à pour dépendance un fichier `Y`, les commandes correspondantes à la cible `X` ne seront exécutées que si et seulement si le fichier `Y` a été modifié depuis la dernière construction de la cible `X`.
La tâche de construction des cibles est donc accélérée du fait que les commandes redondantes ne seront pas lancées.
Dans l’exemple suivant :

1. On invoque `make application`, ce qui compile un fichier `main.c` et en résulte un fichier exécutable `application`.
1. On invoke encore `make application` mais cette fois-ci aucun travail n’est fait.
C’est parce que la date de dernière modification du fichier `main.c` précède la date de dernière modification du fichier `application`.
Il n’est donc pas nécessaire de re-compiler `main.c`.
1. On modifie le fichier `main.c`.
(L’application `sed` opère une recherche et remplacement du mot «Hello» par le mot «Goodbye».)
4. On invoque encore `make application`. 

~~~
$ make application
gcc main.c -o application -lm
$ make application
make: `application' is up to date.
$ sed 's/Hello/Goodbye/' main.c
$ make application
gcc main.c -o application -lm
~~~

## D’autres questions ?

**Q :** Est-ce qu’il y a un modèle à suivre ?  
**R :** Pas strictement mais certaines cibles avec toujours les mêmes noms et tâches sont souvent attendues.
Parmi les cibles «classiques» (c.f. https://www.gnu.org/prep/standards/html_node/Standard-Targets.html) :

- `all` : pour construire toutes les cibles qui produisent des applications.
- `install` : pour installer dans le système d’exploitation le(s) applications(s) et/ou la(es) bibliothèque(s).
- `clean` : pour effacer tous les fichiers générés (fichiers objet, application) et repartir de zéro.
- `check` : pour tester et confirmer que les programmes fonctionnent correctement.

**Q :** Que peuvent-être les cibles ? Et les dépendances ? Et les commandes ?  
**R :** Les cibles sont habituellement des fichiers générés mais ce n’est pas une obligation (voir ci-haut).
Les dépendances sont habituellement des fichiers déjà existants ou d’autres cibles.
Les commandes peuvent être tout ce que l’on peut faire à l’invite de commandes.

**Q :** Il n’y a que `make` qui peut faire ça ?  
**R :** Oh, que non ! Il y a pléthore d’autres outils :

- Outils plus spécialisés ou qui n’opèrent que dans un écosystème plus particulier.
P. ex. : `Gradle` pour Java, `Grunt` pour Javascript, `Rake` pour Ruby, etc.
(c.f. https://en.wikipedia.org/wiki/List_of_build_automation_software).
- Outils propres aux environnements de développement.
P. ex. : MSBuild et fichier `.sln`, Xcode et fichier `.xcodeproj`, etc.
- Outils servant à générer des fichiers `makefile`, `.sln`, `.xcodeproj`, etc.
P. ex. : `configure`, `cmake`, `Meson`, etc. (c.f. https://en.wikipedia.org/wiki/List_of_build_automation_software#Build_script_generation).
Ces outils sont souvent utilisés dans un contexte de code écrit en langage compilé qui doit être exécuté sur différentes plateformes à la différence de code écrit en langage interprété qui peut être exécuté tel quel.

**Q :** Mais, attendez, c’est un peu comme un EDI ?
Notre EDI peut déjà automatiser certaines tâches.  
**R :** C’est pas faux mais `make` n’est pas une application interactive alors que déboguer un programme est une tâche qui demande de l’interaction, ce qu’une application avec une interface graphique nous permet.

**Q :** `make`, c’est vieux !
Pourquoi utiliser `make` dans ce cours ?  
**R :** Du respect pour vos ancêtres !
Et puis, c’est moins vieux que la perceuse électrique...
Plus sérieusement, les divers outils de développement vont et viennent comme autant de vagues de mode mais certains «grands-pères» survivent et continuent d’influencer les nouvelles générations.
Se familiariser avec `make` c’est comme se familiariser avec le latin.

*[démo]*

Nous avons vu :

>un `makefile` avec trois cibles simples :  
>bonjour:
><br>&nbsp;&nbsp;echo Bonjour !
><br><br>
>compilation:
><br>&nbsp;&nbsp;gcc main.c
><br><br>
>executer: compilation
><br>&nbsp;&nbsp;./a.out

> Le programme `make` est le programme qui interprète un `makefile`.

> En invoquant simplement `make` à l’invite de commande, la première cible est exécutée.
Ici, on imprime «Bonjour !».
On obtient le même résultat en spécifiant la cible.
P. ex. : `make bonjour`.

> On peut aussi invoquer les autres cibles avec `make compilation` et `make executer`.

> Nous avons vu aussi que d’invoquer directement la cible `executer` va d’abord compiler notre programme car la cible `executer` dépend de la cible `compilation`.

# Il faut s’organiser

Jusqu’à maintenant vous avez été appelé·e·s à développer des programmes de un ou quelques fichiers.
Professionnellement, ce nombre passera à des dizaines ou des centaines.
Par exemple, le navigateur Firefox est constitué de plus de 300 000 fichiers.
Organiser tout ce code devient rapidement nécessaire.

## Organisation logique

Une première division logique entre divers fichiers d’un programme consiste à séparer le code de présentation et le code de traitement.
Par «présentation» on fait référence à l’[interface avec l’utilisateur](https://fr.wikipedia.org/wiki/Interface_utilisateur) du programme.
Cette interface peut être à l’invite de commandes, à travers une fenêtre graphique ou via un [dispositif haptique](https://fr.wikipedia.org/wiki/Dispositif_haptique).
Par exemple, il est possible de naviguer le web a l’invite de commandes avec [Lynx](https://fr.wikipedia.org/wiki/Lynx_(navigateur)), à travers une fenêtre graphique avec Chrome ou avec un matériel spécialisé offrant un retour de force tactile.
Par contre, quelle que soit la façon dont la page web est rendue, le code qui communique avec le serveur reste le même.

<figure>
    <img src="/assets/images/organisation-logique-separation-interface-traitement.svg"/>
</figure>

Une deuxième division logique s’opère entre le code de traitement et le code des données.
Par «données» on entend les informations requises au bon fonctionnement de l’application qui se trouverait dans une base de donnée, des fichiers de configuration ou des fichiers sauvegardés par l’utilisateur.
Par exemple, vos préférences de votre navigateur web (votre page d’accueil, l’automatisation des mises à jours, l’adresse d’un serveur DNS, etc.) sont sauvegardées dans un fichier de configuration.

<figure>
    <img src="/assets/images/organisation-separation-traitement-donnees.svg"/>
</figure>

Une troisième division logique se fait entre le code de l’application et le code qui teste son bon fonctionnement. 

<figure>
    <img src="/assets/images/organisation-logique-separation-traitement-test.svg"/>
</figure>

En terme de code, ces divisions logiques se traduisent par un regroupement du code par ses responsabilité : le code qui présente des données à l’utilisateur ne devrait pas se retrouver dans la même fonction que le code qui va récupérer ces données ou la dans la fonction qui les transforme.
Plus concrètement, voici un exemple d’un [programme «monolithique»](https://fr.wikipedia.org/wiki/Grande_boule_de_boue) :

~~~ c
int main()
{
    // Lecture des paramètres.
    FILE *db = fopen("parametres.csv", "r");
    if(db)
    {
        char *ligne = NULL;
        size_t taille = 0;
        ssize_t lu;
        if((lu = getline(&ligne, &taille, db)) != -1)
        {
            // Segmentation de la ligne à la virgule.
            for(char* b, *e = ligne; lu; lu -= e - b)
            {
                b = e;
                while(*e != '\0' && *e != ',') ++e;
                if(*e == ',') *e++ = '\0';
                // Présentation du paramètre.
                printf("%s\n", b);
            }
        }
        free(ligne);
        fclose(db);
    }
    return 0;
}
~~~

Tout de suite là, comme ça...
Que fait ce programme ?
Et comment testeriez-vous son bon fonctionnement ?
Voici le même programme avec une [séparation des responsabilités](https://fr.wikipedia.org/wiki/S%C3%A9paration_des_pr%C3%A9occupations) plus claire et évidente :

~~~ c
ssize_t lecture_parametres(char* chemin_fichier, char** ligne)
{
    FILE *db = fopen(chemin_fichier, "r");
    if(!db) return -1;
    size_t taille = 0;
    ssize_t const lu = getline(ligne, &taille, db);
    
    fclose(db);
    return lu;
}

void segmentation(char *ligne, char const separateur)
{
    for(;ligne && *ligne; ++ligne)
    {
        if(*ligne == separateur) *ligne = '\0';
    }
}

void presentation(char const* ligne, ssize_t const n)
{
    for(char const *p = ligne; p && p != ligne + n + 1;)
    {
        p += printf("%s\n", p);
    }
}

int main(int argc, char *argv[])
{
    char *parametres = NULL;
    ssize_t const taille = lecture_parametres("parametres.csv", &parametres);
    segmentation(parametres, ',');
    presentation(parametres, taille);
    free(parametres);
    return 0;
}
~~~

*[démo]*

> Nous avons pu témoigner du fait que ces deux programmes, bien qu’écrit avec du code différent on le même comportement et donne le même résultat.
Cependant, le deuxième programme, du fait de sa bonne division des responsabilités logiques entre des fonctions, est beaucoup plus facile à tester.
De plus, comme les fonctions prennent des paramètres, il est aussi plus facilement modifiable dans le futur si des changements deviennent nécessaires.

Les avantages de cette division logique sont multiples :

1. La vérification de la bonne lecture du ficher avec `if` n’est plus nécessaire.
On simplifie le code et donc la représentation mentale que nous devons nous en donner.
1. Les commentaires ne sont plus nécessaire.
Le nom des fonctions nous racontent le fonctionnement du programme.
1. La fonction `segmentation` est réutilisable.
En faisant abstraction de la fonction de segmentation, on peut maintenant la réutiliser ailleurs.
1. Une meilleure gestion des ressources.
    1. Le fichier qui sert de base de données (`parametres.csv`) est ouvert, lu et refermé dans une seule fonction.
    Les autres fonctions n’ont pas à savoir d’où viennent les données.
    Également, le fichier est refermé sitôt qu’il n’est plus utile plutôt qu’à la toute fin du programme.
    1. La variable `parametres` est gérée par la fonction `main`, là ou elle est crée et utilisée.
1. Une plus grande facilité d’entretien futur.
Ce programme est bien sûr petit mais vous aurez à travailler sur des programmes beaucoup plus gros. 
Chacune de ces fonctions peuvent consister en des dizaines, des centaines, voire des milliers de ligne de code.
Si elles sont entremêlées, y apporter des modifications, des réparations et des améliorations devient très complexe et prends beaucoup plus de temps et d’énergie mentale que nécessaire.
Dans des projets de très grande envergure, les responsabilités d’entretien du code sont même séparées en plusieurs équipes de plusieurs personnes.
1. Une meilleure [testabilité](https://fr.wikipedia.org/wiki/Testabilit%C3%A9).
Le premier programme est difficilement vérifiable.
On ne peut que le lancer et le déboguer commet tel pour vérifier son bon fonctionnement.
La deuxième version, par contre, nous permet d’écrire des tests pour confirmer le bon fonctionnement des fonctions `lecture` et `segmentation`. 

Comment diviserait-on en modules et interfaces :

- Une voiture ?
    - Une voiture autonome et un bus peuvent-ils avoir des modules communs ou interchangeables&nbsp;?
- Une maison ?
- Une ville ?

## Organisation physique

D’une organisation logique découle une organisation physique.
Par organisation physique on entends les répertoires et les fichiers qui constituent notre base de code.
Quand on débute, une base de code ne se constitue souvent que d’un seul fichier.
Pour un programme en C, un fichier intitulé `main.c` suffit.
À mesure que nos programmes se complexifient, il devient pratique de diviser les responsabilités du code en plusieurs fichiers puis en plusieurs répertoires.
Nous pourrions traduire directement la séparation logique du code de notre projet en répertoires correspondant.

~~~
navigateur/
├── présentation/
│   ├── textuelle/
│   │   └── main.c
│   ├── graphique/
│   │   └── main.c
│   └── tactile/
│       └── main.c
├── traitement/
│   ├── réseau/
│   │   └── http.c
│   └── [autre]/
│       └── [autre].c
├── données/
│   └── parametres.c
└── test/
    ├── http.c
    └── parametres.c
~~~

Qu’est-ce ?
Trois fichiers nommés `main.c` ?
N’est-ce pas une erreur ?

Non.
Ceci est une base de code dans laquelle on retrouve le code nécessaire pour produire trois programmes. 
Rappelez-vous que nous parlions de trois interfaces utilisateur différentes pour répondre à des besoins particuliers.
Il est donc possible de lancer trois compilations comme suit :

~~~ shell
$ gcc présentation/textuelle/main.c traitement/réseau/http.c traitement/[autre]/[autre].c données/parametres.c -o nav-text
$ gcc présentation/graphique/main.c traitement/réseau/http.c traitement/[autre]/[autre].c données/parametres.c -o nav-gui
$ gcc présentation/tactile/main.c traitement/réseau/http.c traitement/[autre]/[autre].c données/parametres.c -o nav-tact
~~~

Ces trois programmes réutilisent du code commun au trois.
C’est une très bonne chose mais ces commandes apparaissent un peu répétitive.
Plusieurs techniques ou outils peuvent nous aider à ne pas nous répéter.
Entre autres : le concept de la bibliothèque logicielle et le `makefile`.

## Bibliothèques logicielles

En programmation, une bibliothèque c’est un fichier qui contient des fonctions et des structures prêtes à être utilisées.
Une bibliothèque n’est pas un programme car elle ne contient pas de [point d’entrée](https://fr.wikipedia.org/wiki/Point_d%27entr%C3%A9e) (comme la fonction `int main(int argc, char *argv[])` en C).

Dans notre exemple, les divisions logiques nous donnent une bonne indication d’arrangement de notre code en bibliothèques réutilisable :

~~~ shell
$ gcc traitement/réseau/http.c -o réseau.o
$ ar crs libréseau.a réseau.o
~~~

Le fichier `http.c` n’est dorénavant compilé qu’une seule fois et le ficher `libréseau.a` contient maintenant le code compilé pour communiquer entre deux ordinateurs avec le protocole HTTP.
On peut l’intégrer à un programme comme suit :

~~~ shell
$ gcc présentation/textuelle/main.c -lréseau [...] -o nav-text
$ gcc présentation/graphique/main.c -lréseau [...] -o nav-gui
$ gcc présentation/tactile/main.c -lréseau [...] -o nav-tact
~~~

Dans notre exemple, on ne trouvait que le fichier `http.c` mais on peut imaginer une bibliothèque contenant beaucoup plus de fonctionnalités en lien avec les protocoles de communications : `smtp.c`, `ntp.c`, `ssh.c`, etc.

### Bibliothèques tierce partie

Dans le monde du développement logiciel on ne construit jamais rien complètement par soi-même.
On construit des maison, des manoirs, des châteaux mais on ne fabrique ni le mortier ni les briques. 
Le mortier, c’est les outils comme le compilateur et les briques ce sont des bibliothèques déjà existantes écrites par d’autres développeur·se·s.
Dans tous les écosystèmes de programmation il existe des bibliothèques pour tous les domaines, allant de la cryptographie à la (dé)compression de fichier.

Intégrer dans son programme une bibliothèque déjà existante pour profiter de ses fonctionnalités est toujours une très bonne idée.
La facilité d’intégration dépend de l’écosystème.
C’est souvent plus facile pour les langages interprétés que pour les langages compilés.

Vous utilisez déjà une bibliothèque que vous n’avez pas écrite vous-même, peut-être sans l’avoir réalisé.
Quand, dans un programme C, on appelle la fonction `printf`, le code pré-compilé de cette fonction est dans une bibliothèque logicielle.
Sous Linux Debian, c’est `/usr/lib/x86_64-linux-gnu/libc.so.6`.
Cette bibliothèque contient toutes les fonctions de C «standard» et le compilateur lie votre programme avec sans que vous ayez à le demander explicitement.
On peut visualiser la présence de la fonction `printf` dans le fichier comme suit :

~~~ shell
$ nm /usr/lib/x86_64-linux-gnu/libc.so.6 --dynamic | grep printf
[...]
0000000000055910 T parse_printf_format
0000000000058560 T printf
0000000000108280 T __printf_chk
[...]
~~~

Par contre, quand en C, on appelle des fonctions mathématiques (comme `pow`), on doit explicitement demander au compilateur de lier notre programme avec la bibliothèque qui contient le code pré-compilé de ces fonctions.
Sous Linux Debian, c’est `/usr/lib/x86_64-linux-gnu/libm.so.6` et on la lie avec notre programme avec l’option `-lm`. 

~~~ shell
$ nm /usr/lib/x86_64-linux-gnu/libm.so.6 --dynamic | grep pow
[...]
0000000000021440 W cpowl
000000000000fff0 W pow
000000000000fab0 T pow10
[...]
~~~

### Bibliothèque statique et bibliothèque partagée

Jusqu’à maintenant nous avons vu que les bibliothèques étaient liées avec le programme au moment de la compilation.
Le fichier final du programme se retrouve donc enrichi de la bibliothèque.
C’est un lien avec une bibliothèque [statique](https://fr.wikipedia.org/wiki/Biblioth%C3%A8que_logicielle#La_technique).

Il est possible de lier le programme avec une bibliothèque partagée.
C’est le même code dans la bibliothèque mais le fichier n’est pas ajouté au programme directement.
Il reste au chaud dans les réserves du système d’exploitation et est chargé en mémoire à la demande. 
C’est justement le cas pour les bibliothèques `libc.so.6` et `libm.so.6`.
Comparons avec notre navigateur :

<figure>
    <img src="/assets/images/bibliotheque-statique-partagee.svg"/>
</figure>

Une bibliothèque partagée présente des avantages et des inconvénients.
Les avantages :

1. La taille des programmes est plus petite car ils ne contiennent pas en eux le code en question.
1. Une bibliothèque partagée peut être mise à jour séparément sans avoir à recompiler les programmes.

Les inconvénients :

1. L’utilisateur du programme doit s’assurer qu’elle est présente dans son système.
La bibliothèque partagée doit être installée d’une façon ou d’une autre.
1. Si une mise à jour rend une plus récente version incompatible avec une plus ancienne version, les deux versions doivent être gardées disponibles.

## Compilation croisée

Il y a des projets qui ont comme but d’être utilisés sur plusieurs plateformes différentes.
On peut penser à :

- un jeu qui est disponible à la fois sur Xbox et Playstation.
- un programme comme Word ou Photoshop qui est disponible sous Windows et macOS.
- une appli qu’on peut télécharger pour son iPhone ou son Android.
- un site web qui doit apparaître de la même façon qu’il soit rendu sous Chrome ou Firefox.

Ça, ce sont des exemples de plateformes logicielles (systèmes d’exploitations ou navigateurs web) mais il y a aussi des différence matérielles comme un processeur de PC [Intel](https://en.wikipedia.org/wiki/List_of_Intel_microprocessors) ou un processeur de téléphone mobile [ARM](https://en.wikipedia.org/wiki/ARM_architecture#Market_share).

<figure style="float: right">
    <img src="/assets/images/compilation-croisee.svg"/>
</figure>

(Pourquoi différents processeurs ?
Pour différents besoins.
Par exemple, un PC ayant accès à 800 watts de puissance versus un téléphone mobile avec une batterie fournissant seulement 10 watts.
Pour le PC on privilégie la puissance de travail, pour le téléphone ce sera la durée de vie.)

Pour que du code compilé (contrairement à interprété) fonctionne sous divers environnements d’exécution, il doit être compilé plusieurs fois.
Il doit être compilé *n* fois pour *n* environnements.
Pour ce faire, soit on compile le code dans l’environnement en question, soit on utilise un [compilateur croisé](https://en.wikipedia.org/wiki/Cross_compiler).

# Comment se protéger des autres

## Gestion de version II

<figure style="float: right">
    <img src="/assets/images/add-commit-push-pull.svg"/>
</figure>
Vous serez appelés·es à travailler en équipe sur une même base de code.
Ce code sera hébergé sur un serveur central et les modifications apportées par les différents participants doivent être synchronisés périodiquement.

Si deux ingénieur·e·s travaillent sur des fichiers différents, ça va encore.
Mais que se passe-t-il quand ils doivent modifier le même fichier ?

Reprenons notre lettre à Michèle et donnons-lui rendez-vous.

> On se voit à 13h ?

~~~
H -> I
~~~

| Message | Étape | git |
| - |:-:| - |
| Bonjour Michèle. Comment allez-vous ? Vous n’auriez pas cent euros ? | H | Bonjour Michèle. Comment allez-vous ? Vous n’auriez pas cent euros ? |
| Bonjour Michèle. Comment allez-vous ? On se voit à 13h ? Vous n’auriez pas cent euros ? | I | `+On se voit à 13h ?` |

Seulement nous serons deux à rencontrer Michèle. Alice et Bob sont deux à être responsable de ce message et sont tous deux aptes à y apporter des modifications. Après cette première suggestion, Alice et Bob réalisent que cette heure ne leur convient pas. Alice est disponible à 14h, Bob à 15h.


>On se voit à 13h ?

***
>&nbsp;&nbsp;&nbsp;&nbsp;On se voit à 14h ?

***
>&nbsp;&nbsp;&nbsp;&nbsp;On se voit à 15h ?

~~~
        -> J
      /     \
H -> I       ?
      \     /
        -> K
~~~

Qui de Alice ou Bob gagne ?
La première personne à fusionner ses modifications ?
Ou plutôt la dernière personne car elle écrasera les modifications précédentes ?
Il n’y pas de gagnant, il ne doit y avoir qu’un accord entre tous.
Mais dans les faits, voici ce qui va se dérouler : la première personne à fusionner sa branche dans la branche principale (`master` p. ex.) n’aura aucun problème à le faire puisque le delta sera sans conflit.
Admettons qu’Alice soit plus rapide sur la gâchette :

~~~
        -> J
      /     \
H -> I - - -> L ->  ?
      \            /
        -> K  - -
~~~

| Message | Étape | git |
| - |:-:| - |
| Bonjour Michèle. Comment allez-vous ? Vous n’auriez pas cent euros ? | H | Bonjour Michèle. Comment allez-vous ? Vous n’auriez pas cent euros ? |
| Bonjour Michèle. Comment allez-vous ? On se voit à 13h ? Vous n’auriez pas cent euros ? | I | `+On se voit à 13h ?` |
| Bonjour Michèle. Comment allez-vous ? On se voit à 14h ? Vous n’auriez pas cent euros ? | J | `-13h +14h` |
| Bonjour Michèle. Comment allez-vous ? On se voit à 15h ? Vous n’auriez pas cent euros ? | K | `-13h +15h` |
| Bonjour Michèle. Comment allez-vous ? On se voit à 14h ? Vous n’auriez pas cent euros ? | L | `-13h +14h` |
| | ? | `**-13h**` `+15h` |

*[démo]*

> Nous avons vu ce qui se passe quand deux branches créées sur un même dépôt entrent en conflit.
La première branche à être fusionnée «gagne» et la deuxième fait face à un conflit qui doit être résolu à la main.
Ici, Bob devra résoudre ce conflit avant de pouvoir confirmer son `commit`.
La résolution passera probablement par une discussion avec Alice pour décider d’une heure qui convient à tout le monde...

Au moment où Bob voudra fusionner sa propre branche où il suggère 15h, il ne pourra le faire directement car il y aura conflit.
Un conflit du fait que qu’il n’y a plus de `13h` à effacer et à remplacer par autre chose.
Le point de départ du delta n’existe plus !
`git` (ou tout autre programme de gestion de version) le signalera à Bob et Bob aura à manuellement confirmer ses modifications.
Avant, il aura tôt fait de demander son avis à Alice car ce conflit est un indicateur d’une véritable discordance entre les deux auteurs.

Dans cet exemple, on observe que Alice et Bob peuvent travailler en parallèle.
S’ils ne modifient pas exactement la même ligne, personne n’aurait eu à gérer un conflit.
Par exemple, si Alice change l’heure et que Bob précise le lieu du rendez-vous :

~~~
          -> J
        /     \
H -> I - - -> L -> M
        \          /
          -> K - -
~~~

| Message | Étape | git |
| - |:-:| - |
| Bonjour Michèle. Comment allez-vous ? Vous n’auriez pas cent euros ? | H | Bonjour Michèle. Comment allez-vous ? Vous n’auriez pas cent euros ? |
| Bonjour Michèle. Comment allez-vous ? On se voit à 13h ? Vous n’auriez pas cent euros ? | I | `+On se voit à 13h ?` |
| Bonjour Michèle. Comment allez-vous ? On se voit à 14h ? Vous n’auriez pas cent euros ? | J | `-13h +14h` |
| Bonjour Michèle. Comment allez-vous ? Vous n’auriez pas cent euros ? Rendez-vous au club ! | K | `+Rendez-vous au club !` |
| Bonjour Michèle. Comment allez-vous ? On se voit à 14h ? Vous n’auriez pas cent euros ? | L | `-13h +14h` |
| Bonjour Michèle. Comment allez-vous ? On se voit à 14h ? Vous n’auriez pas cent euros ? Rendez-vous au club ! | M | `+Rendez-vous au club !` |

*[démo]*

> Nous avons vu comment Alice et Bob peuvent, en parallèle, modifier le contenu de la lettre et, tant qu’ils ne modifient pas exactement le même contenu (la même ligne de texte), `git` additionne tous les deltas apportés par Alice et Bob pour arriver au résultat final sans conflits.
Bien sûr, Alice et Bob doivent régulierement revenir sur la branche `master` et la synchroniser avec GitHub pour être chacun au fait des plus récentes modifications de l’autre.

# Tests

On écrit toujours des [tests](https://fr.wikipedia.org/wiki/Test_(informatique)).
Ceux qui ne le font pas sont condamnés à répéter leurs erreurs.
Ceux qui le font sont confiants en leur code.
Car il s’agit bien de cela : avoir confiance que notre code accompli ce qu’il doit accomplir dans les paramètres définis.
Il y a deux moments pour écrire des tests : avant d’écrire la première ligne de code et avant de réparer un bogue. 

## Tester avant d’écrire le code

Quand vous avez vos «entrées» et vos «sorties» bien définies (p. ex. $$x_{(n+1)}=rx_{n}(1-x_{n})$$ ou «opérer une rotation sur un tableau»), vous pouvez écrire des tests avant même d’avoir écrit une seule ligne de code car vous connaissez l’avant et l’après et c’est ce qu’un test vérifie.
Un test vérifie que pour un «avant» donné le programme produit l’«après» correspondant.
Les tests font office de contrat.

Évidemment, sans code fonctionnel, le test échoue mais le contrat existe et il ne reste plus qu’à le remplir, justement en écrivant le code. 

### Qu’est-ce que de bons tests ?

<figure>
    <img src="/assets/images/1-percent-failure-rate.svg"/>
    <figcaption>1% d’utilisateurs par jour qui désinstallent votre appli...</figcaption>
</figure>

Testez les cas les plus attendus et testez aussi les cas extrêmes.
Du code qui fonctionne dans 99% des cas, ça veut dire un utilisateur sur cent qui voudra se faire rembourser !
Après 70 jours d’utilisation quotidienne, vous aurez perdu la moitié de vos clients...

Avec [2,5 milliards d’utilisateurs](https://www.statista.com/statistics/264810/number-of-monthly-active-facebook-users-worldwide/), pour Facebook, les «cas rares» ne sont pas rares du tout.
Il leur est impératif de toujours prendre en compte les cas rares plutôt que de les balayer sous le tapis en pensant que «ça n’arrivera jamais».

Quels serait les cas «ordinaires» et les cas «extrêmes» ou «pathologiques» pour :

- une fonction `bool anagramme(char* gauche, char* droite)` ? 
- un programme qui télécharge une photo sur un site web ?
- le logiciel qui fait fonctionner un laser médical ?

<figure>
    <img src="/assets/images/march%C3%A9-robustesse.svg"/>
    <figcaption>Une vraie question d'ingénierie...</figcaption>
</figure>

Mais on teste jusqu’à quand ?
Oui, plus de tests nous amène à une meilleure [robustesse](https://fr.wikipedia.org/wiki/Robustesse_(ing%C3%A9nierie)) du code.
Mais on s’arrête quand ?
Si notre programme est une fonction de beaucoup de paramètres, voire des centaines.
On ne va tout de même pas tester toutes les combinaisons, non ?
Il faut bien livrer notre programme un jour ou l’autre.
À quel moment êtes vous prêts à rendre votre programme public sachant qu’il contient encore des erreurs dont vous ne connaîtriez pas encore la nature ?
Ça devient une question d’ingénierie...
Oui, plus tôt notre programme est à vendre, au plus notre part de marché initiale sera grande mais au risque de perdre des clients à long terme si notre programme n’est fonctionnel qu’à moitié !

### Le coût d’un bogue

<figure>
    <img src="/assets/images/cout-bogue.svg"/>
    <figcaption>Coût relatif de réparation d’un bogue. Le plus tôt, le mieux.</figcaption>
</figure>

Le coût relatif de la résolution d’une bogue en argent et en heures grandit de façon exponentielle en fonction du moment où le bogue est introduit et le moment où il est découvert. C’est une motivation pour comprendre au mieux les besoins auxquels le programme doit répondre et tester au maximum le plus tôt possible.

*[démo - Réparons ensemble un bogue qui a coûté 125 millions  de dollars et au passage, faisons mieux que* `int` et `float` pour décrire des valeurs numériques.]*

> Nous avons vu un exemple de deux fonctions (`fsin(float radian)` et `fcos(float degre)`) qui étaient très bien testée... en isolation !
Dès lors qu’elles étaient combinées, le résultat était erronné.
C’était une analogie de la cause de la perte du [satellite orbital perdu](https://en.wikipedia.org/wiki/Mars_Climate_Orbiter), faute de tests exhaustifs.

## Tester avant de réparer un bogue

Personne n’est parfait et à une certaine échelle, on ne peux tout prévoir.
Il y aura des cas (rares ou non) qui vous aurons échappé et vous aurez un retour d’un utilisateur qui vous dira : «Quand je passe une chaîne vide, le programme se plante.»

Avant de réparer le code, on écrit le test qui reproduit l’anomalie pour confirmer que nous bien compris les circonstances du bogue.
Ce nouveau test *doit* échouer.
Et ensuite seulement on répare le code pour résoudre le problème.

## Le code passe et les tests restent

Votre code va changer au fil du temps.
Il va évoluer, s’améliorer, intégrer de nouvelles fonctionnalités.
Il est important de ne jamais se séparer des tests déjà.
Leur liste ne va aller qu’en s’allongeant.

Le fait même d’ajouter de nouvelles fonctionnalités peut dévoiler un problème qui avait toujours existé dans le code mais qui n’avait pas été exercé jusqu’à lors.
Un exemple abstrait serait deux fonctions, `foo()` et `bar()` qui, testées individuellement donnent un résultat attendu mais pas lorsque combinées l’une à l’autre.
Dans ce cas, on parle de [régression](https://fr.wikipedia.org/wiki/Test_de_r%C3%A9gression).

# Réusinage et entretien

Rare est le code parfait et immuable.
Une fois votre programme livré, vous aurez immédiatement des idées pour le rendre meilleur : plus performant, plus simple, plus facile à comprendre, etc.
Ou alors, votre code n’était pas à la hauteur de vos attentes car sous la pression du temps, vous avez fait au plus pressant pour obéir aux échéances.
Au moins, il passe les tests.
C’est déjà ça.
Mais vous avez accumulé de la [dette technique](https://fr.wikipedia.org/wiki/Dette_technique).

Un des premiers moyens de simplifier du code est de repérer et de factoriser des répétions :

~~~ c
void debiter_compte(db* comptes, int numero_compte, int montant)
{
    for(compte *c = premier(comptes); c; c = next(comptes, c))
    {
        if(c->numero == numero)
        {
            c->solde -= montant;
        }
    }
}

void crediter_compte(db* comptes, int numero_compte, int montant)
{
    for(compte *c = premier(comptes); c; c = next(comptes, c))
    {
        if(c->numero == numero)
        {
            c->solde += montant;
        }
    }
}

crediter_compte(comptes, 123, 100);
~~~

Voilà du code qui se répète beaucoup et qui pourrait mieux «s’exprimer».
Que fait la boucle exactement ?
Peut on exprimer cette boucle avec des mots plutôt qu’avec du code ? Essayons : 

~~~ c
compte* find(db* comptes, int numero)
{
    for(compte *c = premier(comptes); c; c = next(comptes, c))
    {
        if(c->numero == numero)
        {
            return c;
        }
    }
    return NULL;
}

void debiter_compte(db* comptes, int numero_compte, int montant)
{
    compte *c = find(comptes, numero_compte);
    if(c)
    {
        c->solde -= montant;
    }
}

void crediter_compte(db* comptes, int numero_compte, int montant)
{
    compte *c = find(comptes, numero_compte);
    if(c)
    {
        c->solde += montant;
    }
}

crediter_compte(comptes, 123, 100);
~~~

Oui, il y a, en absolu, plus de lignes de code, seulement le code maintenant ne se répète plus et l’objectif des deux fonctions est moins embrouillées de par leur substance n’étant plus dans une boucle.

Cette factorisation nous amène même à voir plus grand.

~~~ c
void debiter(compte *c, int montant)
{
    c->solde -= montant;
}

void crediter(compte *c, int montant)
{
    c->solde += montant;
}

void operation(db* comptes, int numero, void (*f)(compte*, int), int montant)
{
    compte *c = find(comptes, numero);
    if(c)
    {
        f(c, montant);
    }
}

operation(comptes, 123, credit, 100);
~~~

Maintenant les deux fonctions ont été réduites à leur essence.
L’ensemble est non seulement mieux testable car les différentes parties sont séparées en fonctions qui ne font que ce que leurs noms impliquent mais en plus on pourra plus facilement, dans le futur, définir d’autres opérations sur des comptes.
«Remettre à zéro», par exemple.

*[démo, ROT13 par César]*

> Le bonus 1 individuel vous demande de réusiner (retravailler) votre code pour faire en sorte que les fonctions `chiffre_ROT13` et `chiffre_Cesar` accomplissent leur responsabilités non pas avec du code implémentant directement leur algorithme respectif mais en appelent la fonction `chiffre_Vigenere`.
Ce faisant, vous réduisez la quantité de code écrit et, par la fait même, la possibilité d’erreurs à différents endroits de votre base de code.

# Intégration continue

Auparavant, il était fréquent que le cycle de *développement-test-livraison* s’étirait sur plusieurs jours, voire des semaines.
Cette pratique s’est modernisée en introduisant des outils qui permettent de raccourcir ce cycle et que les développeur·se·s obtiennent un «feedback» beaucoup plus rapide sur la la convenance et la qualité de leur code.
Plus les problèmes sont détectés tôt, plus facilement ils seront corrigés.

L’intégration continue consiste à s’assurer qu’avant qu’une modification soit intégrée dans le logiciel (p. ex. en fusionnant une branche introduisant une nouvelle fonctionnalité dans la branche `master`), tous les tests existant soient exécutés de manière indépendante et impartiale.
Du fait, le système d’IC est habituellement un serveur qui simule l’intégration du nouveau code dans la base de code courante et exécute tous les tests et autres scripts qui s’assurent que le programme reste fonctionnel et dans un état «livrable».

Mais si le développeur s’assure d’exécuter les tests avant d’intégrer son code, pourquoi le faire une deuxième fois ?

### Parce que vous allez oublier

Ça va arriver.
C’est inévitable.
Vous allez oublier de rouler les tests et vous allez provoquer une catastrophe qui va bloquer tous vos coéquipiers jusqu’à ce que vous rétractiez vos changements.
L’automatisation vous évite cette situation.

Un autre scénario est que vous allez rouler les tests diligemment et puis vous allez apporter une toute petite dernière modification «qui peut pas faire de mal» à votre code...
Elle va faire du mal.

### «Ça fonctionne sur mon ordinateur !»

Super.
Malheureusement ce n’est pas ça qui compte.
Ce qui importe c’est que ça fonctionne sur la «cible type».
Du code qui fonctionne sous Linux peut ne pas même compiler sous Windows ou iOS.
Il n’est pas faisable de faire en sorte que tous les développeurs d’une équipe testent leurs modifications en tout temps sur toutes les plateformes requises.
Un système d’IC peut offrir cette possibilité.

Il est même possible que votre code contienne un bogue qui ne se manifeste pas 100% du temps.
Tester votre programme sur un autre ordinateur augmente vos chances de découvrir ce bogue.

### Vous ne pouvez pas tout faire

Un système d’IC peut offrir d’autre paramètres d’exécution difficilement reproductible sur votre propre ordinateur.
Deux exemples : simuler un réseau de mauvaise qualité ou simuler un ordinateur moins performant que le vôtre.
(Aussi étonnant que cela puisse paraître, bien des bogues peuvent resté «caché» parce que le code est exécuté à une certaine vitesse).

### Parce que ça prendrais trop de temps

Certaines base de code ont des suites de tests très élargies qui peuvent durer plusieurs heures !
Un système d’IC va répartir ces tests entre plusieurs machines ce qui divisera le temps d’exécution.

*[démo]*

> Nous avons vu qu’au moment d’ouvrir un «Pull Request» pour l’exercice 3, GitHub lance une machine virtuelle qui fait exactement ce nous faison nous-même localement pour compiler et tester notre programme.
Le fichier de configuration qui dicte cette action à GitHub est `./github/workflows/test-master.yml`.

# Documentation

En tant qu’ingénieur·e, vous avez à produire deux types de documentation pour deux publics différents.
Comme tout bon orateur, il faut adapter votre discours à votre audience.

## Documentation pour l’utilisateur·trice

Ceci constitue le manuel d’instructions de votre programme et, dépendamment du profil de vos utilisateurs, vous aurez à adapter la technicité de votre langage.
Ces instructions peuvent prendre aussi différentes formes.
Par exemple, un vidéo tutoriel vaut mille mots d’explication abstraite, surtout pour des programmes très interactifs.
Pour certains jeux, c’est même l’évolution du joueur dans un premier niveau avec un environnement contrôlé qui fait office de documentation.
Le joueur «apprend en faisant».

Cette documentation doit couvrir les interactions et les erreurs les plus fréquentes.

Des exemples de documentations pour l’utilisateur que vous avez déjà consultées en tant que développeur·se ?
Peut-être https://docs.python.org/ ou https://cppreference.com ?

## Documentation pour l’ingénieur·e

Ceci constitue le manuel d’entretien de votre programme.
Le client, c’est vous.
Cette documentation est bien sûre plus technique et doit vous permettre de revenir rapidement sur vos pas et de comprendre la structure du système et le pourquoi du comment.
Ceci afin de pouvoir rapidement résoudre un problème ou d’améliorer votre programme.

Il y a plusieurs degrés ou niveaux de documentation.

### Haut niveau

Ici sont décrites les responsabilités des divers modules et composants de votre programme ainsi que les relations entre ces modules et composants.
Pour décrire et documenter l’organisation logique des composants d’un programme, on utilise des diagrammes.
Souvent, ce sont des diagrammes [UML](https://fr.wikipedia.org/wiki/UML_(informatique)).
Tous les diagrammes du document que vous lisez présentement sont essentiellement des diagrammes UML.

<figure>
    <img src="/assets/images/high-level-ex2.svg"/>
    <figcaption>Diagramme UML des composants et modules de l’exercice 2.</figcaption>
</figure>

### Bas niveau

Essentiellement, ce seront vos commentaires dans votre code qui peuvent être assemblés en un seul document qui vous aideront vous, ou un·e futur·e collègue de travail à comprendre pourquoi et comment le code fait ce qu’il fait.

# Stades de développement

Entre le moment où vous écrivez la première ligne de code de votre programme et le moment où vous avez votre premier·ère utilisateur·trice, votre base de code code passera par plusieurs [degrés de maturité](https://en.wikipedia.org/wiki/Software_release_life_cycle).

Un moyen de communiquer la maturité d’un programme est par sa version.
La numérotation de version la plus répandue suit le schéma «X.Y.Z».
Cependant, l’exacte signification des chiffres n’est pas universelle.
Le standard le plus connu est décrit par les [spécifications de gestion sémantique](https://semver.org/lang/fr/) où :

1. X représente le numéro de version majeur.
Il est incrémenté quand sont introduits des changements non-compatibles avec la version majeure courante.
1. Y représente le numéro de version mineur.
Il est incrémenté quand sont introduits de nouvelles fonctionnalités compatibles avec la version majeure courante.
1. Z représente le numéro de correctif.
Il est incrémenté quand sont introduits des corrections de bogues ou autres.

Ce ne sont pas tous les projets logiciels qui ont besoin d’une version.
Un site web, par exemple, peut changer à tous les jours et donner une version sémantique n’aurait que peu de sens.

## Pre-alpha (version 0.*n*.0 ou 1.0.0-alpha.*n*+pre)

### Spécifications fonctionnelles

Cette phase commence par une analyse et une description détaillée des besoins et des exigences du futur utilisateur.
Ces besoins et exigences sont documentés et servent de référence pour le reste du développement à chaque fois que vous vous poserez une question sur le «quoi».
Ces spécifications fonctionnelles décrivent le «quoi», pas le «comment», et servent de communication entre l’équipe de développeur·se·s et les clients.

Par exemple, pour la conception d’une voiture : «Plusieurs conducteurs doivent-ils être en mesure de mettre en mémoire leurs positions de conduite individuelles ?»
Pour une voiture de luxe, la réponse à cette question est «oui» et les sièges doivent doit être conçus et fabriqués en conséquences.
Pour une voiture plus économique, cette fonctionnalité n’est pas requise et la réponse est «non».
Si le document d’analyse ne répond pas à une question, révisez-le.

### Spécifications de conception

À cette étape, on analyse les besoins et les exigences techniques du programme.
La division des responsabilités en modules, le type de base de données à utiliser, le langage de programmation à employer, les bibliothèques tierce partie à intégrer, l’interface utilisateur, etc.
Ces spécifications de conception décrivent le «comment» et servent de communication au sein de l’équipe de développeur·se·s.

Comment les conducteurs vont ajuster leur siège ?
Par un bouton sur le tableau de bord ?
Ou un mécanisme sous le siège ? 

### Développement

Viens ensuite la programmation comme telle où, petit à petit, on écrit le code qui satisfait les exigences du problème.
Pendant cette phase, il est important de construire progressivement le programme et de suppléer chaque ajout de fonctions par des tests associés.

À ce stade, les fonctions ou modules qui compose le programme sont [testés séparément](https://fr.wikipedia.org/wiki/Test_unitaire), ce sont des tests unitaires.
Le programme n’est pas encore prêt à être testé dans son ensemble.

Pour notre exemple de voiture, à ce stade nous avons un moteur, testé en isolation et un système de freinage aussi testé en isolation, etc.

## Alpha (version 1.0.0-alpha.*n*)

Le programme se tient suffisamment debout pour être testé [dans son ensemble](https://fr.wikipedia.org/wiki/Test_d%27int%C3%A9gration) même s’il peut encore contenir des bogues, voire des bogues catastrophiques.
Le programme est testé par les développeur·se·s et/ou des testeur·se·s en interne.

Pour notre exemple de voiture, à ce stade nous avons ce qui ressemble à une voiture comme on en connaît et qui est testée comme telle.
Elle sera en premier testée par les concepteurs et ensuite par des pilotes expérimentés.

À la fin de cette phase, le programme contient toutes les fonctionnalités demandées et aucune nouvelle [fonctionnalité ne sera ajouté](https://fr.wikipedia.org/wiki/Feature_freeze) à partir de ce moment. 

## Beta (version 1.0.0-beta.*n*)

Le programme continu d’être testé en interne mais il aussi testé en externe par des testeurs ou un groupe choisi d’utilisateurs potentiels.
On teste aussi sa [facilité d’utilisation](https://fr.wikipedia.org/wiki/Test_utilisateur) ou son ergonomie.

Pour notre exemple de voiture, nous demanderons à des acheteurs potentiels de nous donner leurs avis sur sa tenue de route, son ergonomie, son confort, etc.

Les bogues à résoudre en priorité sont ceux les plus visibles par les utilisateurs.

## Livraison (version 1.0.0)

Vous avez atteint le moment où vous avez suffisamment confiance en votre code et votre programme pour le mettre en marché pour qu’il soit utilisé. 

Champagne !
Profitez-en, ça ne dure qu’un après-midi.

## Support

Après la sortie publique d’un logiciel, l’auteur·e peut (ou doit, si un contrat le stipule) offrir un support technique.
Pendant cette phase, le programme continu à être testé pour de potentielles [régressions](https://fr.wikipedia.org/wiki/Test_de_r%C3%A9gression).
C’est-à-dire qu’en aucun cas on ne doit ré-introduire d’anciens bogues résolu en essayant de réparer des bogues existants ou d’ajouter de nouvelles fonctionnalités.

### Correctifs (version 1.0.*n*)

Personne n’est à l’abri de bogues découverts une fois le logiciel officiellement livré.
Quand des bogues sont toujours présents dans la version 1.0.0 et que ces bogues affectent le bon fonctionnement du programme, on y apporte des [correctifs](https://fr.wikipedia.org/wiki/Patch_(informatique)).
Ce ne sont que des modifications qui ajuste ou corrige l’implémentation des fonctionnalités déjà existantes dans le programme.

### Ajouts rétrocompatibles (version 1.*n*.0)

On peut aussi ajouter de nouvelles fonctionnalités compatibles avec une version antérieur du programme.

Par exemple, un programme d’édition d’image peut se voir ajouter de nouvelles fonctions sans pour autant rendre les fichiers enregistrés auparavant incompatibles avec cette nouvelle version.
C’est-à-dire qu’un fichier créé et enregistré par la version 1.0.0 du programme est chargeable par la version 1.1.0.
On s’attend aussi à ce que cette compatibilité soit [réciproque](https://fr.wikipedia.org/wiki/Compatibilit%C3%A9_ascendante_et_descendante).

Par contre, si les ajouts sont de telle envergure que les fichiers enregistrés par la version 1.0.0 sont inutilisables, c’est qu’il est temps d’appeler cette nouvelle version «2.0.0».

## Versions subséquentes (version *N*.0.0)

Les besoins de vos client changeront avec le temps.
Ce qui était important avant ne l’est plus et vice-versa ou encore vous avez trouvé comment faire encore mieux en intégrant de nouvelles technologies ou systèmes.
Pour quelque raisons que ce soit, il est temps de penser à la prochaine version de votre programme.

Le cycle de développement recommence en partant des [spécifications fonctionnelles](https://www.dropbox.com/scl/fi/12l29vxc1v4z74wum6ay3/D-velopper-comme-les-pros.paper?dl=0&rlkey=gbd3b2ajnlo93wz6xvsph5bcu#:h2=Sp%C3%A9cifications-fonctionnelles).


## Gestion de version III

Pour de petits programmes produits en solitaire, travailler sur une unique branche (`master`) peut être suffisant. Pour des projets de plus grande envergure qui regroupent plusieurs équipes ou qui auront plusieurs versions, on voit souvent utiliser des branches de longue durée. Le modèle suivi dépend de chaque équipe mais un exemple courant ressemble à ceci :

~~~
A - - -> D - - - - - - -> G - - -> J - - -> M
 \      / \              / \      / \      / \
  B -> C   \            /   H -> I   K -> L   \
          [1.0] - -> [1.1] -> ...            [2.0] -> ...
            \      /
             E -> F
~~~

Dans cet exemple, le commit  `D` correspond au moment ou la version 1.0 était finalisée.
À partir de cette étape commence le développement de la version 2.0 qui survient au commit `M`.
On remarque aussi que la version 1.0 s’est vue évoluer vers une version 1.1.

# «J’ai trouvé du code sur l’Internet»

## Licences

**Q :** J’ai trouvé de la prose sur Internet.
Ai-je le droit de la copier-coller dans ma dissertation ?
- C’est pas écrit « tous droits réservés »...
- Il n’y a pas de symbole «©»
- Essentiellement, tout ce qu’on trouve sur Internet c’est gratuit... Non ?  
**R :** Sauf indication contraire, les droits d’auteurs s’appliquent et sa reproduction doit faire l’objet d’une demande explicite.
Si elle est entré dans le domaine public, sa reproduction est libre mais l’auteur doit être cité.

**Q :** J’ai trouvé du code sur Internet.
Ai-je le droit de copier-coller ce code dans mon programme ?
- Il n’y a pas d’indications contraires évidentes...
- L’auteur semble vouloir nous aider...
- C’est que trois lignes de code...
- Essentiellement, tout ce qu’on trouve sur  Internet c’est gratuit... Non ?  
**R :** Voir réponse précédente.

«Sauf indication contraire» ?
Oui, il existe des licences qui permettent la reproduction de code et l’intégration de ce code dans un programme.
On appelle ce code du [logiciel libre](https://fr.wikipedia.org/wiki/Logiciel_libre).

## Libre vs propriétaire

Le logiciel développé en entreprise est rarement libre, il est alors «[propriétaire](https://fr.wikipedia.org/wiki/Logiciel_propri%C3%A9taire)».
Il appartient à l’entreprise et elle seule décide qu’en faire car elle en retient tous les droits.
Le logiciel libre, par opposition, offre à son utilisateur [quatre libertés](http://www.gnu.org/philosophy/free-sw.fr.html) :

- Exécuter le programme, pour tous les usages.
- Étudier le fonctionnement du programme et de l'adapter à ses besoins.
- Redistribuer des copies du programme, gratuitement ou pas.
- Améliorer le programme et de distribuer ces améliorations.

Les deuxième et quatrième libertés impliquent que le code source du logiciel soit disponible (ouvertement ou sur demande).

## «Free» ≢ «free» («libre» ≢ «gratuit»)

Il est important de distinguer la disponibilité du code d’une application ou d’une bibliothèque logicielle et sa gratuité.
La corrélation entre les deux existe très souvent mais elle n’est pas automatique.
Ce n’est pas un lien de causalité.

Pour preuve, on trouve sur l’Internet des «[freewares](https://fr.wikipedia.org/wiki/Freeware)», des applications gratuites ou des jeux gratuits pour plateformes mobiles mais dont le code source n’est pas disponible.
Inversement, il existe des [licences «shared source»](https://fr.wikipedia.org/wiki/Shared_Source) qui donne accès au code source mais qui restreignent les droits de redistribution et de vente.

## D’autres questions ?

**Q :** Donc, je peux ré-utiliser ce code que j’ai trouvé sur l’Internet dans mon programme ?  
**R :** Oui si sa licence est une licence de logiciel libre.
Soyez attentifs à aussi garder la licence en question intacte et de la redistribuer avec le code.
Une licence de logiciel libre ne vous permet pas de vous arroger la paternité du logiciel.

**Q** :  Quel est la licence de code sur http://stackoverflow.com ?  
**R** : Le code sur StackOverflow est CC BY-SA 3.0: https://creativecommons.org/licenses/by-sa/3.0/fr/

**Q :** Est-ce que je dois mettre mon code sur l’Internet ?  
**R :** Sans avoir à en faire son hobby, pour des étudiants ou des professionnels, avoir du code consultable en ligne qui porte son nom est une bonne façon de démontrer ce qu’on est capable de faire et complémente favorablement le C.V.
Ce code peut être un projet qui nous est propre ou des [contributions](https://up-for-grabs.net/#/) à un projet déjà existant.

**Q :** Est-ce que je peux mettre mon code dans le domaine public ?  
**R :** Bien sûr. [Voici](https://choosealicense.com/licenses/unlicense/) votre licence.

**Q :** Pourquoi mettre en ligne du code sans être payé ?
On peut pas gagner sa vie comme ça.  
**R :** Il existe [plusieurs modèles économiques](https://fr.wikipedia.org/wiki/Mod%C3%A8les_%C3%A9conomiques_des_logiciels_open_source) ayant pour base le logiciel libre.
Par exemple, du code peut avoir une double licence, une libre et une à des fins commerciales.
On peut demander des dons ou faire payer des services connexes comme du support technique ou une formation.
Une compagnie peut aussi payer pour des modifications propres à ses besoins.

# Et après ?

Bienvenu dans le monde du développement de logiciel où vous ne devriez jamais arrêter d’apprendre.
Un monde où les outils et les langages sont en constante évolution, les sources d’informations ne se tarissent jamais et les conférences sont [pléthores](https://confs.tech/?topics=&countries=France).

# Lexique

Il se peut aussi que le professeur utilise des mots d’anglais pendant les cours.
Ce n’est pas pour vous rendre la vie difficile, c’est comme ça qu’il a travaillé pendant longtemps !
Et comme l’anglais est la lingua franca de la programmation, vous verrez souvent ces termes anglais plus souvent utilisés que leurs équivalents français.

| Anglais | Français |
| - | - |
| [breakpoint](https://en.wikipedia.org/wiki/Breakpoint) | [point d’arrêt](https://fr.wikipedia.org/wiki/Point_d%27arr%C3%AAt_(informatique)) |
| [codebase](https://en.wikipedia.org/wiki/Codebase) | [base de code](https://fr.wikipedia.org/wiki/Codebase) |
| [integrated development environment (IDE)](https://en.wikipedia.org/wiki/Integrated_development_environment) | [environnement de développement intégré (EDI)](https://fr.wikipedia.org/wiki/Environnement_de_d%C3%A9veloppement) |
| [library](https://en.wikipedia.org/wiki/Library_(computing)) | [bibliothèque logicielle](https://fr.wikipedia.org/wiki/Biblioth%C3%A8que_logicielle) |
| [log](https://en.wikipedia.org/wiki/Log_file) | [journal](https://fr.wikipedia.org/wiki/Historique_(informatique)) |
| [package](https://en.wikipedia.org/wiki/Package_format) | [paquet](https://fr.wikipedia.org/wiki/Paquet_(logiciel)) |
| [patch](https://en.wikipedia.org/wiki/Patch_(computing)) | [correctif](https://fr.wikipedia.org/wiki/Patch_(informatique)) |
| [refactoring](https://en.wikipedia.org/wiki/Code_refactoring) | [réusinage](https://fr.wikipedia.org/wiki/R%C3%A9usinage_de_code) |
| [repo, repository](https://en.wikipedia.org/wiki/Repository_(version_control)) | [dépôt](https://fr.wikipedia.org/wiki/D%C3%A9p%C3%B4t_(informatique)#D%C3%A9p%C3%B4t_de_code_source) |
| [stack trace](https://en.wikipedia.org/wiki/Stack_trace) | [trace d’appels](https://fr.wikipedia.org/wiki/Trace_d%27appels) |
| [revision control](https://en.wikipedia.org/wiki/Version_control) | [gestion de version](https://fr.wikipedia.org/wiki/Gestion_de_versions) |
| [version control](https://en.wikipedia.org/wiki/Version_control) | [gestion de version](https://fr.wikipedia.org/wiki/Gestion_de_versions) |

# Varia

Une collection d’histoires de débogage racontées par des développeur·se·s : <https://github.com/danluu/debugging-stories>

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
