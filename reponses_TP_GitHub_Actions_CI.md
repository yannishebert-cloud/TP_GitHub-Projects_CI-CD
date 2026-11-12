# TP — Découvrir l'intégration continue avec GitHub Actions : réponses aux questions

Les réponses ci-dessous sont les réponses attendues. Pour les parties qui dépendent de ce que vous observez à l'écran (noms exacts des tests, journaux GitHub), adaptez-les à vos propres résultats.

## Question 1 : `git init`, `git add`, `git commit`

- **`git init`** crée le dossier caché `.git` : le projet devient un dépôt Git local, qui peut suivre l'historique des modifications. Rien n'est encore enregistré.
- **`git add`** place des fichiers dans la zone de préparation (l'index). Cela choisit ce qui entrera dans le prochain commit, sans rien enregistrer définitivement.
- **`git commit`** enregistre l'état de l'index comme un instantané permanent dans l'historique, avec un message décrivant la modification.

## Question 2

1. **Dépôt local** : la copie du projet et de son historique sur votre ordinateur, sur laquelle vous travaillez. **Dépôt distant** : la copie hébergée sur un serveur (ici GitHub), partagée et accessible aux autres.
2. **`origin`** est simplement le nom par défaut donné à l'URL du dépôt distant. Il pourrait porter n'importe quel autre nom, mais `origin` est la convention.
3. **`git push`** envoie vos commits locaux vers le dépôt distant pour mettre à jour la branche correspondante.
4. **`-u`** (pour *upstream*) lie la branche locale `main` à la branche `origin/main`. Ensuite, `git push` et `git pull` suffisent sans préciser le dépôt ni la branche. C'est ce lien que `git branch -vv` affiche.

## Question 3

**Intérêt de l'environnement virtuel** : il isole les bibliothèques du projet de celles du système et des autres projets. On évite ainsi les conflits de versions et on peut reproduire l'installation à l'identique. La CI, qui repart d'une machine propre, illustre le même principe : elle installe uniquement ce que déclare `requirements.txt`.

**Division par zéro** : le test ne vérifie pas que le programme réussit, mais qu'il lève bien une exception précise. Il encadre l'appel avec `pytest.raises(ZeroDivisionError)`. Si l'exception attendue est levée, le test réussit. Le programme « lève une exception » est donc le comportement voulu.

## Question 4

Le workflow contient `on: push: branches: [main]`. GitHub déclenche donc le workflow à chaque push sur `main`, que la branche soit liée à une pull request ou non. Le déclencheur porte sur l'événement *push* et sur la branche, pas sur l'existence d'une PR. Le premier `git push -u origin main` correspond exactement à ce cas.

## Question 5

**Pourquoi le test détecte la régression** : avec `return a - b`, `add(a, b)` ne renvoie plus la somme attendue. L'assertion de `test_add` compare alors deux valeurs différentes et échoue. Par exemple, pour `add(2, 3)`, la valeur attendue est `5` et la valeur obtenue `-1`. Le test vérifie un comportement précis, et ce comportement a changé.

**Ce que le journal apporte que l'icône rouge ne montre pas** : l'icône indique seulement qu'un contrôle a échoué. Le journal donne le nom exact du test en échec, la valeur attendue et la valeur obtenue, l'étape qui a échoué, le code de sortie et la commande exécutée. C'est lui qui permet de comprendre la cause et de corriger, sans devoir deviner.
