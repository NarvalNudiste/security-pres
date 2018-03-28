# Virus

### Définition

* Programme se repliquant lui-même
  * Mauvaise utilisation du terme
* Utilisation de failles de sécurité / humaines
* Des motivations multiples
  * Politiques
  * Militantes
  * Mais bon en général c'est quand même pour l'argent
* Coûts importants
  * 5 milliards en 2017 (estimés) juste à cause des Ransomwares

### Premiers travaux académiques

* 1949, par John von Neumann
* "Theory of self-reproducing automata"
  * Neumann décrit comment un programme pouvait être designé pour se repliquer lui même
  * Ecrit en assembleur sur un Siemens 4004
  * Considéré comme le premier virus
  * Neumann considéré comme le papa de la virologie informatique
* Creeper
  * Ecrit par Bob Thomas, a infectés des systèmes TENEX
  * Accès par Arpanet
  * "I'm the creeper, catch me if you can!"
* Core War
  * des devs se tirent la bourrent sur quel programme va effacer les autres
  * en profitant du multiproc et des changements de contexte

### Operations et fonctions

* Méchanisme d'infection
  * Routine de recherche, renvoie une liste de fichiers à infecter
* Trigger
  * Trigger : Bombe logique, moment où le virus s'active
  * Peut être une date, une heure précise, le moment où un disque devient plein, un moment oû un autre soft précis est présent aux côtés du virus, etc.
* Payload
  * Ce qu'il fait, pas forcément méchant ça dépends du but
* Phases correspondantes
  * Phase dormante
    * Dormante : Le virus a réussi à s'introduire, mais il fait rien
    * tous les virus n'ont pas cette phase
    * on attends le trigger
  * Phase de propagation
    * Insère une copie de lui-même dans des autres softs, logiquement
    * Chaque programme infecté contient un clone du virus, on appelle ça la phase de propagation
  * Phase de trigger
  * Phase d'execution
      * Trigger + exec à peu près la même chose, c'est quand ça fout la merde


### Types de virus

* Resident / Non-résident
  * Résident : Memory resident virus, reste dans la ram depuis le boot de la machine jusqu'à qu'on l'éteigne .. Peut intercepter des demandes de l'os et changer le flow au module de réplication (???????)
  * Non résident : scan le disque, infecte et se barre
* File infector viruses
  * Infectes les programmes
  * Typiquement des exe
  * Souvent Memory Residents
* Boot sector viruses
  * Résidents par Nature
  * Boot sector virus : s'attache au boot sector  
* Master boot record viruses
  * Similaire aux boot sector
  * Peu d'info dessus, concernait surtout Windows NT qui bootaient plus quand infectés
* Multipartites virus
  * Les records + les exe, dégats particulièrement durs à réparer
  * Les deux se re-infectent en permanence entre eux
* Macro
  * Au lieu d'infecter des exe, on infecte des données
  * Le plus commun et le plus couteux au final
  * Office 97
  * Infecte Word, Excel, PowerPoint et Access
  * Peut concerner potentiellement tous les programmes qui ont des languages de programmation internes

### Invisibilité (*Stealth techniques*)

* Vieilles méthodes
  * La plupart des virus essayent de pas trop se faire gauler
  * Exemple bête mais sur ms-dos ils faisaient gaffe au fait que le "last modified" du fichier hôte soit pas touché
  * .. Mais ça marche pas hyper bien vu que certains antivirus gardent justement une trace de ces dates de façon cyclique
  * *cavity virus (Tchernobyl)*
  * *kill antivirus avant (Conficker)*
* Read requests intercepts
  * une fois que l'infection est faite, difficile de clean le systeme
  * NTFS -> propriétaire donc antivirus est obligé d'envoyer une read request
  * Le virus trick les software antivirus en choppant la requête, en faisant le boulot à sa place et en retournant un fichier non infecté
  * L'interception se fait par injection de code dans le système qui gère les read requests
  * l'antivirus peut pas savoir ce qui s'est passé et reçoit des trucs cleans
  * Pour contrer ça on peut booter sur une machine externe clean en isolant les fichiers
  * On compare les installs de windows en calculant leur hash avec celles d'origine pour détecter les fichiers modifiés.
  * Hashs de fichiers windows dans des db
  * Dans les vieilles versions, des hash crypto étaient directement placées dans windows
  * mais pourri parce que ces mêmes hashs stockés pouvaient être modifiés

* Self-modification

  * Les antivirus modernes cherchent les virus avec leur signature
  * Mais le terme joue pas puisque les virus ont pas de signature unique comme les humains
  * Recherche de séquence de byte connues pour être des parties de virus
  * Si on trouve un pattern on cherche des autres fichiers parce que pas sûr que c'est un virus (faux positif)
  * Du coup, modification du code à chaque fichier infecté
  * par contre le code qui se modifie lui même c'est rare et c'est suffisant pour qu'un antivirus tagge le truc comme suspicieux
  
* Virus encryptés

  * Pour éviter la détection on peut encrypter le corps du virus
  * en gardant le module d'encryption et une clé en clair stockée quelque part
  * Du coup le seul truc qui reste constante est le module de décryptage * (si le virus est encrypté avec une clé différente à chaque fichier infecté)
  * plus possible de les trouver avec des signatures mais possible de détecter le module d'encryption
  * Plus un module d'encryption / decryption dans une signature c'est un peu chelou

* Polymorphique

  * Première technique à vraiment poser des soucis aux scanners de virus
  * Comme les virus encryptés, mais on modifie aussi le module d'encryption/decryption
  * Si bien écrit, aucune partie du code ne reste
  * Certains virus ralentissent l'infection, en s'empêchant de muter si il infecte un ordi qui contient déjà des copies de virus, du cccoup
  * ils ont besoin d'avoir un moteur polymorphique à l'intérieur

* Métamorphisme
  * Se réécrive entièrement à chaque fois
  * Need un moteur de metamorphisme
  * Par ex W32/Simmile est métamorphique mais sur ses 14'000 lignes d'assembleur, 90% sont dédiées au moteur

### Vulnérabilités

* Bugs software
  * Quand c'est du fromage c'est la fête
* Social engineering / Mauvaises pratiques de sécurité
  * Pour se repliquer il doit obtenir la permission de s'executer et d'écrire dans la mémoire...
  * Du coup ils essaient de s'injecter dans des executables légitimes
  * Extensions cachées
  * 5.66 % de toutes les vulnerabilités
* Vulnérabilités OS

### Défense

* Antivirus
  * detecte et éliminie les virus connus
  * mise à jour duh (holes)
  * n'empêche pas un virus de se transmettre
  * deux grandes méthodes :
    * liste de signatures, on regarde le contenu de la ram et du boot sector (zero day attack)
    * algorithmes heuristiques basés sur les comportements communs de virus
    * faux positifs possibles
 * Backups lol
 * Etre intelligent

### Quelques virus notables

* Tchernobyl
  * Résident
  * Première detection en juin 1998
  * Ne sévit que le 26 août 1998, date anniversaire de Tchernobyl
  * Versions de windows et jeux infectés circulaient sur internet
  * Ecrase le premier megaoctet de chaque disque dur (MBR), tente d'effacer le BIOS
  * Récupération compliquée
  * Si BIOS effacé c'est mort, autant racheter une carte mère

* MyDoom.A (2004)
  * mail / kazaa
  * s'envoie au carnet d'adresse et installe une backdoor
  * envoie aussi à des adresses random -> on peut rediriger ces adresses vers une vraie après
  * autorise la prise de contrôle (backdoor SHIMGAPI.DLL)

* Cabir (2004)
  * Sybian OS
  * se propage par bluetooth
  * CARIBE.SIS
  * Affiche "Caribe" à l'écran, rien de dangereux
  * Proof-of-concept
