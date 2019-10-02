# Victor Magat - TP4

## Exercice 1. Gestion des utilisateurs et des groupes

**1) Commencez par créer deux groupes groupe1 et groupe2**

On utilise les commandes suivantes pour créer deux groupes :
`sudo addgroup groupe1` et `sudo addgroup groupe2`

**2) Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de
leur dossier personnel et avec bash pour shell**

Les 4 commandes suivantes permettent de créer 4 utilisateurs en ajoutant un dossier personnel et avec bash pour shell :

- `sudo useradd -m -s /bin/bash u1`<br>
- `sudo useradd -m -s /bin/bash u2`<br>
- `sudo useradd -m -s /bin/bash u3`<br>
- `sudo useradd -m -s /bin/bash u4`<br>

**3) Placez les utilisateurs dans les groupes :**
- **u1, u2, u4 dans groupe1**
- **u2, u3, u4 dans groupe2**

Les commandes suivantes permettent de placer les utilisateurs dans les groupes suivants :
- `sudo adduser u1 groupe1`
- `sudo adduser u2 groupe1`
- `sudo adduser u4 groupe1`
- `sudo adduser u2 groupe2`
- `sudo adduser u3 groupe2`
- `sudo adduser u4 groupe2`

**4) Donnez deux moyens d’afficher les membres de groupe2**

Deux moyens d'afficher les membres du groupe2 sont :

- `cat /etc/group | grep groupe2`
- `getent group groupe2`

**5) Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire
de /home/u3 et /home/u4**

Les 4 commandes suivantes permettent de changer le groupe propriétaire des répertoires personnels des utilisateurs suivants :

- `sudo chgrp -R groupe1 /home/u1`
- `sudo chgrp -R groupe1 /home/u2`
- `sudo chgrp -R groupe2 /home/u3`
- `sudo chgrp -R groupe2 /home/u4`

**6) Remplacez le groupe primaire des utilisateurs :**
- **groupe1 pour u1 et u2**
- **groupe2 pour u3 et u4**

Pour remplacer le groupe primaire des utilisateurs, il faut exécuter les commandes suivantes :

- `sudo usermod u1 -g groupe1`
- `sudo usermod u2 -g groupe1`
- `sudo usermod u3 -g groupe2`
- `sudo usermod u4 -g groupe2`

**7) Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et
mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier
associé.**

Les deux premières commandes permettent de créer le dossier commun aux groupes, puis les deux suivantes, de définir le droit d'écriture seulement pour les utilisateurs du groupe.

- `sudo mkdir /home/groupe1`
- `sudo mkdir /home/groupe2`
- `sudo chmod 020 /home/groupe1`
- `sudo chmod 020 /home/groupe2`

**8) Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer
ou supprimer ce fichier ?**

Il suffit de compléter le premier bit dans notre commande comme ceci :

- `sudo chmod 1020 /home/groupe1`
- `sudo chmod 1020 /home/groupe2`

**9) Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?**

Non car le compte n'est pas activé, c'est à dire que nous n'avons pas donné de mot de passe à u1

**10) Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son
compte.**

Grâce à `sudo passwd u1` nous pouvons définir un mot de passe pour l'utilisateur u1. Nous pouvons maintenant nous connecter avec `su u1`.

**11) Quels sont l’uid et le gid de u1 ?**

Nous obtenons ces informations grâce à la commande `id u1`

**12) Quel utilisateur a pour uid 1003 ?**

Nous pouvons exécuter cette commande `cat /etc/passwd | grep x:1003` car nous savons que la 3e colonne correspond à l'uid et que le mot de passe a été chiffré (d'où la présence du **x**).

**13) Quel est l’id du groupe groupe1 ?**

Avec la commande `getent group groupe2 | cut -d : -f 3` on obtient l'id du groupe groupe1 car nous savons que l'id d'un groupe se trouve dans la 3e colonne.

**14) Quel groupe a pour guid 1002 ? (Rien n’empêche d’avoir un groupe dont le nom serait 1002...)**

Il faut exécuter cette commande afin de trouver le groupe ayant un guid 1002 `getent group | grep x:1002`

**15) Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.**

On ne peux pas, car le groupe groupe2 correspond au groupe primaire de l'utilisateur u3.

**16) Modifiez le compte de u4 de sorte que :**
- **il expire au 1
er juin 2019**
- **il faut changer de mot de passe avant 90 jours**
- **il faut attendre 5 jours pour modifier un mot de passe**
- **l’utilisateur est averti 14 jours avant l’expiration de son mot de passe**
- **le compte sera bloqué 30 jours après expiration du mot de passe**

Les commandes suivantes répondent aux questions précédentes respectivement dans le même ordre :

- `sudo chage -E 06/01/2019 u4`
- `sudo chage -M 90 u4`
- `sudo chage -m 5 u4`
- `sudo chage -W 14 u4`
- `sudo chage -I 30 u4`

**17) Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?**

`cat /etc/passwd | grep root | cut -d : -f 7` permet de trouver le shell de l'utilisateur root.

**18) à quoi correspond l’utilisateur nobody ?**

nobody est un utilisateur dont aucun fichier n'appartient et qui n'est dans aucun groupe. Il peut être utilisé pour des demon, pour limiter les risques, pour les serveurs.

**19) Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ?
Quelle commande permet de forcer sudo à oublier votre mot de passe ?**

Il faut ajouter l'option **timestamp_timeout=XX** (en minutes) à côté de la variable **env_reset** dans le fichier accessible via la commande `sudo  visudo`

## Exercice 2. Gestion des permissions

**1) Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques
lignes de texte. Quels sont les droits sur test et fichier ?**

Les lignes de commandes suivantes permettent de répondre aux questions précédentes :

- `mkdir test`
- `cd test`
- `touch fichier`

Les permissions sont :

- **drwxrwxr-x** pour le dossier test
- **-rw-rw-r--** pour le fichier fichier


**2) Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en
tant que root. Conclusion ?**

`chmod 000 fichier` permet de retirer tous les droits sur ce fichier. Aucun changement avec root, il passe outre les permissions du fichier.

**3) Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo
Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un
fichier s’il existe déjà. Que peut-on dire au sujet des droits ?**

La commande `chmod 300 fichier` donne les droits d'écriture et d'exécution au fichier seulement pour nous.

**4) Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.**

Il faut avoir les droits de lecture pour pouvoir exécuter le fichier car le shell a besoin de lire le contenu. Cela fonctionne avec sudo.

**5) Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le
contenu du répertoire, puis exécutez ou affichez le contenu du fichier fichier. Qu’en déduisez-vous ?
Rétablissez le droit en lecture sur test**

Nous n'avons plus les permissions de lister le contenu du répertoire ou de lire le fichier.

**6) Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au
répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit
en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvez-vous déduire de toutes ces manipulations ?**

Impossible de modifier le nouveau fichier peu importe les droits du dossier test. Le fichier peut être supprimer après demande de confirmation.

**7) Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test.
Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en
lister le contenu, etc…Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?**

Il est impossible de créer, supprimer ou modifier un fichier dans le répertoire test mais aussi de s'y déplacer; j'en déduis donc que le droit d'exécution sur un dossier est obligatoire et nécessaire.

**8) Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui
à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire
test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des
droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd
..” ? Pouvez-vous donner une explication ?**

J'ai les mêmes résultats que pour la question précédente. J'en conclus que l'influence des droits que l'on possède sur le répertoire courant est la même que sur n'importe quel répertoire. Oui nous pouvons retourner dans le dossier parent car nous avons le droit d'exécution sur le dossier parent.

**9) Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants
pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.**

`chmod 340 fichier` permet de définir le droit de lecture pour les membres du groupe.

**10) Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture,
ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.**

`umask 0077` pour restreindre les droits seulement à moi-même pour tout le contenu du dossier.

**11) Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.**

`umask 0022` pour restreindre les droits en écriture à moi-même et en lecture et exécution pour tout le monde pour tout le contenu du dossier.

**12) Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux
membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.**

`umask 0037` qui nous permet de toute faire et restreint les membres du groupe à seulement la permission de lecture sur tout le dossier.

**13) Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous
pourrez vous aider de la commande stat pour valider vos réponses) :**
- **chmod u=rx,g=wx,o=r fic**
- **chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x---**
- **chmod 653 fic en sachant que les droits initiaux de fic sont 711**
- **chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r--r-x---**

Les transcriptions sont les suivantes :

- `chmod 534 fic`
- `chmod 602 fic`
- `chmod u=rw,g=rx,o=wx fic`
- `chmod 520 fic`

**14) Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier
/etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?**

`ls -l /usr/bin/passwd` pour afficher les droits du programme passwd, on a `-rwsr-xr-x`.
`ls -l /etc/passwd` pour afficher les droits du fichier passwd, on a `-rw-r--r--`.

**15) Access Control Lists (ACL) : suivez le tutoriel de cette page : https://doc.ubuntu-fr.org/acl.**

**16) Quotas disques : suivez le tutoriel de cette page : https://doc.ubuntu-fr.org/quota.**
