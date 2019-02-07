# Source Control Management

Source Control Management is an important component in our continuous integration and continous deployment pipeline.

It is an huge part of the software development workflow in daily basis:
- Each coding evoluated by th
- Tout code evolue par les developpeurs sera trackes dans le Source Control Management
- Les developpeurs utiliseront le Source Control Management pour tracquer leur changement separement (via des branches) et puis les merger ensemble.

Toutes part d'automatisations qui a besoin d'interagir avec le code source utilisera le Source Control Management
- L'integration continue va obtenir le code du Source Control Management
- Le Source Control Management va notifier le Serveur de Control d'integration quand le code aura besoin d'etre builde.


J'ai utilise Git et plus particulierement GutHub, pour gerer le code source et le versionning

Create a new branch
```console
$ git checkout -b new_feature
```

Create a pull request:
```console
$ git add web/new_feature.py
$ git commit -m "new_feature"
$ git push origin new_feature

```
