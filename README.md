﻿# Tutoriel développement site avec ASP.NET core 1.1 EF6

1. Introduction
2. Étapes de développement
3. Définition concepts ASP.NET core
3. Hoster le site
4. Référence

## Introduction
---

Ceci est un guide détaillé dans lequel je batis un site avec ASP.NET core
de facon structurer en expliquant les concepts de ce framework. Aussi dans
la dernière section j'explique en détail comment configurer un serveur de
développement avec Arch Linux pour toute les composentes du site web.

Le site qui sera développer est un intranet pour une entreprise de publicité.

### Compétence Nécessaire

* Language C# dans le framework .NET peux importe la version
* System.Thread.Task et System.Linq 
* Connaissance en Base de Donnée relationnel
* Connaissance en Linux si vous voulez un serveur de développement
... si vous aimez payez de l'argent et sauvez du temps vous pouvez
... utilser Azure à la place mais il n'en sera pas question ici

### Configuration votre ordinateur

Avant de commencer voici les possibilités d'application pour développer le projet.

Pour travailler sur un project ASP.NET core il existe plusieurs posibilité en terme
d'IDE et d'os. Personnellement, je travail dans Visual Studio Code car j'adore sa lègerté
comparé à Visual Studio et vu que je suis souvent sur Linux j'ai adopté Visual Studio Code
comme IDE principal mais je conseille tous de même Visual Studio pour son intellisens supérieure
dans les pages razor principalement aussi il est plus simple de crée un project avec Visual Studio
qui génère beaucoup du code pour nous. Les liens pour les téléchargers sont dans la section référence.

Dans installer .Net core sur votre machine, veuillez consulter le liens suivant [Téléchargement .net core](https://www.microsoft.com/net/download/core)
Vous devriez retrouver un liens pour votre système d'exploitation. Certaine distribution linux comme Arch Linux même si elles ne sont pas directement
listé possède un packet maintenue par la communauté [AUR .Net Core](https://aur.archlinux.org/packages/dotnet/) 

## Étapes de développement du site web intranet
---

Dans cette section qui est le coeur du tutoriel, je vais construire mon site Web en deux phases dinstincts.
Premièrement, un cours plan du style UML dans lequel je vais construire une shéma de la base de donnés requis
et lister les fonctionnalités a implémenter dans le site web. Deuxièmement, je vais sauté au code pour mettre en
action la conception faite à l'étape précédente.

### UML

### Définition du l'application

Des clients qui possèdent une entreprise de publicité souhaite se faire développer un site web du style intranet
Pour pouvoir intéragir avec une base de donnée qui contienderait les entreprises avec lesquels ils fonts afferts
et des banques d'informations d'entreprises dans les régions sous leur juridications avec lesquels ils pourraient
communiquer pour vendre de la publicité ou permettre d'en acheter au commercant. Les contact avec les nouvelles
entreprises sont effectués par les Vendeurs qui sont des employés qui doit scheduler des rencontres et ensuite une
fois effectuer doivent rédiger un rapport. Pour que le vendeur sache qu'elle entreprise contacter, un des
administrateurs peux assigner des rencentres déja prévu ou informer un vendeur qu'il doit essayer de contacter
une liste d'entreprise donné dans un certain laps de temps.

Les clients ont aussi mentionnés que des services d'envoies d'email automatique vers les vendeurs et les entreprises
serait utilses anssi qu'il possède dèja des fichiers excels avec des contacts d'entreprise et aimerais pouvoir importer
et exporter des documents excel à partir du site web.

Bien sur vu que le site est un intranet et qu'il possède plusieurs roles(admin,vendeur) il doit incorporer un système
AAA et doit pouvoir être servit publiquement de facon sécuritaire (SSL)


#### Fonctionnalité nécessaire

* Authentification pour atteindre l'intérieur du site web
* Restriction de certaine page selon le role dans l'entreprise (vendeur,administrateur)

Commun:
* Faire des Query dans les différents tables de la base de donnée selon des Models prédéfinis
.. pour nos Vues.
* Modifier les informations de leur compte (mdp,information)
* Scheduler une rencontre avec une entreprise, ajouter entreprise et ses informations
* Envoyer des emails prédéfinis a des contacts d'entreprise

Pour Vendeur:
* Consulter son "Dashbord" pour voires c'est tâches assigner comme les entreprise a contacter
... et c'est rencontres à venir.
* Afficher un rapport pour une certaine periode de temps(exemple pour les 3 prochain jours):
    * D'entreprise a rencontrer
    * De rapport à écrire
    * D'entreprise à essayer de contacter

Pour Administrateur:
* Menu assigner tâche de Contacter une entreprise a un vendeur
* Crée des pool d'entreprise à contacter dans une Région pour les vendeurs de cette région
* Ajouter des employes , régions et vendeurs
* Crée des fichiers de recontres du type (???) qu'il peut exporter ou envoyer par email
... à un pool d'adresse email (par exemple tous c'est vendeurs)
* Importer des documents excel dans remplir la base de donnée si le skel de ce fichier
... est connue par le site.


#### Conception base de donnée

Liste des Tables:
* Adresse : contient une adresse
* ContactInfo : contient les informations pour contacter une personne (email,telephone,...)
* Employer : compte d'un employé pour se connecter dans l'intranet
* Région : contient les régions dans lequels ils ont des entreprises
* VendeurInfo : tables lié a un employer qui est vendeur pour contenir c'est rencontres et rapports
* Entreprise : contient les informations des entreprises 
* ContactEntreprise : lié a une entreprise contient la personne qu'on peut contacter dans l'entreprise
* Rencontre : lié a une entreprise contient un evenement pour une rencontre
* Rapport : lié a une rencontre , il s'agit du rapport de la rencontre écris par chaque vendeur après la rencontre


### Application

#### 1) Création du project

Pour commencer le développement, je vais crée un projet avec Visual Studio 2017 qui permet d'intégré à la création les
objects nécessaire pour utiliser le framework Identity pour l'authentification auquel je vais revenir plus tard dans la
section 4. Lors de la création du project il est possible de choisir API , MVC ou Vide. Bien entendu ici je vais chosir
de crée une application MVC qui va crée une base avec des controllers de base déja crée. Si vous n'avez pas Visual Studio
il est aussi possible de crée des projects de base avec l'outil de commande dotnet. Vous pouvez voire la liste possible
avec la commande suivante :

```bash
λ dotnet new --help
[...]
Templates                 Short Name      Language      Tags
----------------------------------------------------------------------
Console Application       console         [C#], F#      Common/Console
Class library             classlib        [C#], F#      Common/Library
Unit Test Project         mstest          [C#], F#      Test/MSTest
xUnit Test Project        xunit           [C#], F#      Test/xUnit
ASP.NET Core Empty        web             [C#]          Web/Empty
ASP.NET Core Web App      mvc             [C#], F#      Web/MVC
ASP.NET Core Web API      webapi          [C#]          Web/WebAPI
Solution File             sln                           Solution

Examples:
    dotnet new mvc --auth None --framework netcoreapp1.1
    dotnet new console --framework netcoreapp1.1
    dotnet new --help

```

Pour des templates plus avancé l'outils generator-aspnet de Yeoman permet de crée des project sur toute les plateformes
il fait partis du project Omnisharp vous pouvez allez vous dans les références pour le lien du github pour des informations
pour comment l'installer sur votre machine et l'utiliser, il y a aussi une video de ASP.NET Monsters sur le sujet.

Avant de passé au reste si vous n'êtes pas famillier avec la structure de l'application veuiller allez voire la section
sur se suject plus bas avant de passer au reste.

#### 2) Ajout des libraires nécessaire

La première vrai étape de code à écrire est d'ajouter les dépendences nécessaire pour le projet. Il y a plusieurs facon
pour les installer mais ici je vais tous simplement les ajouter manuellement dans mon fichier .csproj.

Je vais ajouter premièrement la librairie Npgsql pour EF core
```xml
<PackageReference Include="Npgsql.EntityFrameworkCore.PostgreSQL" Version="1.1.0" />
<PackageReference Include="Npgsql.EntityFrameworkCore.PostgreSQL.Design" Version="1.1.0" />
```
Anssi qu'un port de EPPlus sur .NET core pour travailler avec des documents spreadcheat excel
```xml
<PackageReference Include="EPPlus.Core" Version="1.3.2" />
```

Lorsqu'on ajoute des librairies, pour les ajouter on utilise la commande
*dotnet restore* dans le même repertoire que notre fichier.csproj pour
télécharger les dépendences nécessaire pour l'application. Dans un IDE
comme VS code ou visual studio les changements sont détecter automatiquement et executer en arrière plan.

#### 3) Création des objects de notre model EF6


#### 4) Configuration ASP.NET core Identity

#### 3) Crée le context pour notre base de donnée

#### 4) Crée des repo dependency injection pour travailler le context

#### 5) Création de controller pour nos models

#### 6) Créations Filtres de validation

#### 6) Définition de nos Vues nécessaire

#### 7) Création de model pour passer à nos vues

#### 8) Création de View Component/Tag Helper pour générer le contenue dans les vues

#### 9) Création des vues et ajustement des controller

#### 10) Test unitaire pour intégraton continue

#### 11) Sécurisation


## Définition composentes ASP.NET core 1.1
---

Pour commencer, l'avantages principales qu'on accorde à ASP.NET core MVC est qu'il est construit de
facon très modulaire en utilisant des objects qui hérite d'interfaces ou de classe abstraite qu'on
peut réutiliser pour réimplenter certaine composente selon nos besoins. Mais pour faire cela, il faut
avoir une bonne connaissance du Framework et des différentes intéractions qui ont lieux dérrière.


### Structure d'un project

La structure est lourd est grosse mais en respectant la conventions dans le nom des fichiers
du project permet d'utiliser des fonctions de facon implicite dans les controleurs et de faciliter
le routage.

Voici la liste des dossiers dans mon project :
* Controllers/
... Contient les objects qui s'occupents de répondre au request
* Data/
... Contient les objects qui travail avec la base de données pour nos models
* Models/
... Contient les objects de nos models qui seront retourner aux vues et/ou sauvegarder
... dans la base de donnée
* Properties/
... Contient les fichiers de configuration pour l'application comme les paramètres qui sont passé
... selon l'environnement de production.
* Services/
... Repertoire pour définir nos service qui seront injecter dans l'application comme un service d'envoi
... de courriel
* Views/
... Contient les views que nos controller vous utiliser pour render les pages webs et les retourner au client
* Infrastructure/
... Contient nos Tag Helpers et View Components
* Filters/
... Contient nos filtres
* wwwroot/
... Répertoire root de l'application web qui contient les fichiers statiques comme nos fichiers CSS et JavaScript
* .bowerrc and bower.json
... Fichier de configuration pour bower, un package manager pour le web qui permet d'ajouter des libraires Jquery/bootsrap par
... exemple dans notre web root
* .appsettings.*
... Contient des configs qui sont passé a l'application au démarrage, on peux en définir pour différent environnement 
* bundleconfig.json
... Ajouter par défault dans notre project il s'agit du fichier de configuration pour le "Bundling and minification" il s'agit
... de technique pour améliorer performance du chargement des pages web en "bundling"/combinant plusieurs fichier css/js ensemble
... allez en lire d'avantage ici : [Bundling and minification](https://docs.microsoft.com/en-us/aspnet/core/client-side/bundling-and-minification)
* *.csproj
... Fichier xml qui contient les informations de build et les dépendences du project à télécharger pour executer le project,
... aussi dans se fichier les target possible sont mentionné(os)
* Program.cs
... Si vous le saviez pas notre application n'est qu'une application console et la main thread est démarrer ici avec les modules
... voulu spécifier
* Startup.cs
... Fichier qui contient la classe qui est lancé au lancement de l'application pour configurer ses routage et ses services.

### MVC

### Entity Framework 6

### Logging

### Tag Helpers

### View Components

### Front-end avec ASP.NET




## Hoster le site web
---

Pour le développement du site web, je vais installer un système de Arch Linux avec tous les logiciels
nécessaire pour faire rouler, debuger et service un site web ASP.NET core.

Je vais crée une machine virtuelle avec VirtualBox pour travailler sur mon site et pour contenir la base de donnée.
Cette VM roulera dans un cloud privé dans dans laquel moi et des collaborateurs vont pouvoir tester le site et heberger
la dernière version de master sur le port 80/443 servit par Nginx et faire rouler des tests d'intégrations continus.
Je vais configurer le système pour permettre de soit travailler sur le serveur ou localement sur son ordinateur et de
seulement utilser la base de donné ou même tous simplement rien du tout et travaillé localement une fois la branch du
project cloner.

Voici la listes des pacquets nécessaire

Pacman :
* base unzip vim curl wget zsh grml-zsh-config tmux grub openssh git iptables (Logiciels de bases)
* nginx certbot-nginx (Serveur Web)
* postgresql (Base de donné)

AUR:
* lttng-ust
* dotnet
* dotnet-cli
* dotnet-sdk

Pip:
* pgadmin4


### Installation du système de base


## Référence
---


#### Doc

Documentation/Tutoriel officiel de Microsoft pour développer application ASP.NET core
[Documentation ASP.NET core Microsoft](https://docs.microsoft.com/en-us/aspnet/core/)

Rérerence des API de .NET, très utile contient souvent des exemples d'utilisations pour les objects du framework
[API reference .NET](https://docs.microsoft.com/en-us/dotnet/api/)

Packet manager pour .NET, si vous voulez chercher les libraires qui existe pour .NET faite attention pas tous
est disponible dans .NET core 1.1
[Packet Nuget](https://www.nuget.org/packages/DotNetCore)

Tutoriel exclusivement sur Entity Framework 6 et explication de son fonctionnement avec les Base de Données
[Tutoriel EF 6](http://www.entityframeworktutorial.net/EntityFramework5/entity-framework5-introduction.aspx)

Guide de Microsoft sur comment bien construire c'est API pour le web
[API Design](https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design)

Différents guide sur comment structurer différents type d'application web
[Guide Architecture Web de microsoft](https://www.microsoft.com/net/architecture)

Guide pour permettre le deboguage à distance avec une machine Linux avec une connection SSH
[Debug .NET core par SSh](https://blogs.msdn.microsoft.com/visualstudioalm/2017/01/26/debugging-net-core-on-unix-over-ssh/)

Generateur de project pour ASP.NET core open source, très utile si on n'a pas Visual Studio et permet de crée c'est propre
template d'application
[generator-aspnet](https://gthub.com/omnisharp/generator-aspnet#readme)

Omnisharp est un project OpenSource pour permettre le développement en C# .NET sur toute les plateformes et dans une
multitude d'editeur( Je l'utilse dans VS Code)
[Open Source intellisence -> omnisharp](https://github.om/omnisharp/)

Project de 69 exemple d'application ASP.NET core, traite plus du routing et middleware que de MVC
[69 samples aspnetcore](https://github.com/dodyg/practical-aspnetcore)

Liste de librairie, outils et framework pour .NET
[Awesome .Net](https://github.com/quozd/awesome-dotnet)

Séries de tutoriel video sur ASP.NTE core sur channel 9 un site de podcast/blogging par des employés de Microsoft pour Microsoft
[ASP.NET Monsters](https://channel9.msdn.com/Series/aspnetmonsters)

#### Application

[Visual Studio Code](https://code.visualstudio.com/)

[Visual Studio Community](https://www.visualstudio.com/vs/community/)

### Livre

Excellent livre qui suit en détail le développement d'une application ASP.NET core MVC
une versione plus en détail de ce que je fais for some $$$$
[Livre PRO ASP.NET Core MVC](http://www.apress.com/us/book/9781484203989)