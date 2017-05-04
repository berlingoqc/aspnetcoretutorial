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

### Définition du l'application ( cas fictif )

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

#### Conception base de donnée


### Application

#### 1) Création du project

#### 2) Ajout des libraires nécessaire

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

### MVC

### Entity Framework 6

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
* postgresql (Base de donné postgresql

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

[Documentation ASP.NET core Microsoft](https://docs.microsoft.com/en-us/aspnet/core/)

[API reference .NET](https://docs.microsoft.com/en-us/dotnet/api/)

[Packet Nuget](https://www.nuget.org/packages/DotNetCore)

[Tutoriel EF 6](http://www.entityframeworktutorial.net/EntityFramework5/entity-framework5-introduction.aspx)

[API Design](https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design)

[Guide Architecture Web de microsoft](https://www.microsoft.com/net/architecture)

[Debug .NET core par SSh](https://blogs.msdn.microsoft.com/visualstudioalm/2017/01/26/debugging-net-core-on-unix-over-ssh/)

[generator-aspnet](https://github.com/omnisharp/generator-aspnet#readme)

[Open Source intellisence -> omnisharp](https://github.com/omnisharp/)

[69 samples aspnetcore](https://github.com/dodyg/practical-aspnetcore)

[Awesome .Net](https://github.com/quozd/awesome-dotnet)

[ASP.NET Monsters](https://channel9.msdn.com/Series/aspnetmonsters)


#### Application

[Visual Studio Code](https://code.visualstudio.com/)

[Visual Studio Community](https://www.visualstudio.com/vs/community/)



### Livre

[Livre PRO ASP.NET Core MVC](http://www.apress.com/us/book/9781484203989)