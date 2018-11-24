Build Automation est l'automatisation des taches necessaires pour processer et preparer le code source au deploiement en production. C'est un important composant de l'integration continue.

Ceci inclus:

la compilation
la gestion des dependences. S'il y a des parties tiers ou des librairies qui ont besoin d'etre present pour compiler, ou tester le code. Ils vont etre inclus dans le meme package.
l'execution des tests d'automatisations. Il s'assure que des tests sont executes et s'ils echouent, cela fait echouer le build
le packagage de l'application pour le deploiement
J'ai utilise Maven comme outil d'automatisation de build

