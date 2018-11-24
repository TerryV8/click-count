Source Control Management est un important composant dans la pipeline d'integration continue et de deploiement continu. C'est une part importante au quotidien du workflow des developpeurs:

Tout code evolue par les developpeurs sera trackes dans le Source Control Management
Les developpeurs utiliseront le Source Control Management pour tracquer leur changement separement (via des branches) et puis les merger ensemble.
Toutes part d'automatisations qui a besoin d'interagir avec le code source utilisera le Source Control Management

L'integration continue va obtenir le code du Source Control Management
Le Source Control Management va notifier le Serveur de Control d'integration quand le code aura besoin d'etre builde.
J'ai utilise Git et plus particulierement GutHub, pour gerer le code source et le versionning
