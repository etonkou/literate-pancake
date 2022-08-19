# Le décorateur @property en Python : Cas d’usage et avantages

Dans cet article, vous apprendrez à travailler avec le décorateur @*property* en Python.

Vous découvrirez :

1. Les avantages des propriétés
2. Les fonctions de bases du décorateur, ce qu’elles sont et comment elles sont liées au décorateur @*property*
3. Comment utiliser @property pour définir les *getters*, *setters* et *deleters*.

## 1. **Contexte et avantages**

Voyons le contexte pour lequel les propriétés sont utilisés en python :

Python n'a pas vraiment de méthodes et d'attributs privés puisque vous pouvez toujours accéder aux éléments qui commencent par un ou deux tirets de soulignement. Cependant, par convention, lorsqu'une variable d'instance ou une méthode commence par un ou deux caractères de soulignement, elle est considérée comme privée. Si vous utilisez ces types de variables et de méthodes, votre code risque de se casser lorsque ces variables/méthodes changent ou seront utilisées dans de nouvelles versions.

Un "***getter***" fait référence à une fonction que vous utilisez pour accéder indirectement à un attribut qui est privé alors qu’un "***setter***" est une fonction que vous utilisez pour définir un attribut privé.

Python n'a pas vraiment ce concept car vous pouvez accéder et modifier un attribut directement dans la plupart des cas.

Les propriétés Python sont un moyen de transformer une fonction de classe en attribut. Elles sont utilisées parce que :

- La syntaxe utilisée pour les définir est très concise et lisible
- On peut accéder aux attributs d'instance exactement comme s'il s'agissait d'attributs publics tout en utilisant les getters et setters pour valider les nouvelles valeurs et éviter d'accéder ou de modifier les données directement.
- En utilisant le décorateur @*property*, on peut réutiliser le nom d'une propriété pour éviter de créer de nouveaux noms pour les *getters*, *setters* et *deleters*.

Ces avantages font des propriétés un outil vraiment génial pour vous aider à écrire un code plus concis et plus lisible.

## 2.  **Les décorateurs**

Une fonction décoratrice est essentiellement une fonction qui ajoute une nouvelle fonctionnalité à une fonction qui lui est passée en argument.

Elle permet d'ajouter une nouvelle fonctionnalité à une fonction existante sans la modifier.

Python est livré avec plusieurs décorateurs intégrés. Il en existe trois que vous pouvez utiliser sans en importer (*classmethod, staticmethod et property*).

Après avoir fait une brève introduction sur les décorateurs, nous parlons dans cet article uniquement du décorateur @ *property* avec un cas d’exemple.

Une fonction peut être décorée en utilisant une syntaxe spéciale. Cette syntaxe spéciale est le signe @ suivi du nom du décorateur.

Ce code est placé avant la définition de la fonction que l’on souhaite décorer.

L’exemple suivant montre à quoi cela ressemble :
```python
def decorator(f):
    def new_function():
        print("Extra Functionality")
        f()
    return new_function

@decorator
def initial_function():
    print("Initial Functionality")

initial_function()
```
Analysons le code ci-dessus :

- En premier lieu, nous découvrons la fonction décoratrice def   `decorator(f)` qui prend une fonction f en argument.

```python
def decorator(f):
    def new_function():
        print("Extra Functionality")
        f()
    return new_function
  ```  

- Cette fonction décoratrice possède une fonction imbriquée, `new_function`.   

Remarquez comment `f` est appelé à l'intérieur de `new_function` pour obtenir la même fonctionnalité tout en ajoutant une nouvelle fonctionnalité avant l'appel de la fonction (nous pourrions également ajouter une nouvelle fonctionnalité après l'appel de la fonction).

- La fonction décoratrice elle-même renvoie la fonction imbriquée `new_function`.

- Ensuite, on trouve la fonction qui sera décorée : `initial_function`. Remarquez la syntaxe très particulière de `@decorator` au-dessus de l'en-tête de la fonction.
```python
@decorator
def initial_function():
    print("Initial Functionality")

initial_function()
```

L’exécution de ce code donne le résultat suivant :
```
Extra Functionality
Initial Functionality
```
Remarquez comment la fonction du décoratrice s'exécute même si nous n'appelons que `initial_function()`. Le fait d’avoir ajouté `@decorator` produit ce résultat.

Après avoir vu tout cela, l’on est en droit de se poser la question : quel est le rapport avec le décorateur *@property* ?

En effet, @property est un décorateur intégré à la fonction *property()* de Python. Il est utilisé pour donner une fonctionnalité "spéciale" à certaines méthodes afin qu'elles agissent comme des *getters*, *setters* ou *deleters* lorsque nous définissons des propriétés dans une classe.

Maintenant que vous êtes familiarisé avec les décorateurs, voyons un scénario réel de l'utilisation de @property

## 3. Cas d’utilisation du décorateur @property

Supposons que nous ayons une classe ***Maison*** qui pour l’instant n’a qu’un seul attribut ***prix*** défini.
```python
class Maison:
	def __init__(self, prix):
		self.prix = prix
```

Cet attribut est public car son nom ne comporte pas de trait de soulignement.

Puisque l'attribut est actuellement public, il est très probable que vous ou tout autre développeur accède et modifie l'attribut directement dans d'autres parties du programme en utilisant la notation par points, comme ceci :
```python
# Accès a la valeur
obj.prix
# Modification de la valeur
obj.prix = 5000000
```
`obj` représente ici la variable qui fait référence a l’instance de la classe **Maison**.

Tout va bien jusqu’à présent.  Maintenant, disons que l’on souhaite faire de cet attribut, un attribut protégé (privé) et aussi de valider la nouvelle valeur avant de l’assigner. Plus précisément, nous souhaiterons vérifier si la valeur est un nombre positif. Comment cela va se faire ?

### 3.1 **- Faisons quelques modifications sur notre code** :

A ce niveau, chaque ligne de code qui accède à la valeur de l'attribut ou la modifie devra être modifiée pour appeler le *getter* ou le *setter*, respectivement. Sinon, le code sera cassé.
```python
# Changer a partir de obj.prix
obj.get_prix()
# Changer a partir de obj.prix = 5000000
obj.set_prix(5000000)
```

C’est en effet à ce niveau que les propriétés montrent leur point fort. 

Avec le décorateur @property, vous n'aurez plus besoin de modifier ces lignes car vous pourrez ajouter des *getters* et des *setters* "en coulisse" sans affecter la syntaxe que vous utilisiez pour accéder ou modifier l'attribut lorsqu'il était public.

Génial, n’est-ce pas ?

### 3.2 - **@property: Syntaxe**

Si l’on décide d’utiliser @property, notre classe ressemblera à l'exemple ci-dessous :
```python
class Maison:

	def __init__(self, prix):
		self._prix = prix

	@property
	def prix(self):
		return self._prix
	
	@prix.setter
	def prix(self, nouveau_prix):
		if nouveau_prix > 0 and isinstance(nouveau_prix, float):
			self._prix = nouveau_prix
		else:
			print("Please enter a valid prix")

	@prix.deleter
	def prix(self):
		del self._prix
```
Plus précisément, on peut définir trois méthodes pour une propriété :

- Un **getter** - pour accéder à la valeur de l'attribut.
- Un **setter** - pour définir la valeur de l'attribut.
- Un **deleter** - pour supprimer l'attribut de l'instance.

Il faut noter que le **prix** est maintenant "**protégé**" en d’autre "**privé**"

L'attribut prix est maintenant considéré comme " **privé** " parce que nous avons ajouté un trait de soulignement à son nom dans `self._prix` :

```
self._prix = prix
```

En Python, par convention, lorsque vous ajoutez un trait de soulignement à un nom, vous indiquez aux autres développeurs qu'il ne doit pas être accédé ou modifié directement en dehors de la classe. Il ne doit être accessible que par l’intermédiaire de *getters* et *setters* s'ils sont disponibles.

### a) **Getter**

Le code ci-dessous montre la méthode getter:
```python
@property
def prix(self):
	return self._prix
```
- `@property` : est utilisé pour indiquer que nous allons définir une propriété. Remarquez comment cela améliore immédiatement la lisibilité car nous pouvons clairement voir le but de cette méthode.
- `def prix(self)` : L'en-tête. Remarquez comment le getter est nommé exactement comme la propriété que nous définissons : `prix` . C'est le nom que nous utiliserons pour accéder et modifier l'attribut en dehors de la classe. La méthode ne prend qu'un seul paramètre formel, *self*, qui est une référence à l'instance.
- `self._prix` : Cette ligne est exactement ce que vous attendez d'un *getter* normal. La valeur de l'attribut privé est retournée.

Voici un exemple de l'utilisation de la méthode *getter* :	

```python
>>> maMaison = Maison(6000000.0)  # Instanciation
>>> maMaison.prix                 # Accès a la value
6000000.0
```
Remarquez comment nous accédons à l'attribut prix comme s'il s'agissait d'un attribut public. Nous ne changeons pas du tout la syntaxe, mais nous utilisons en fait le getter comme intermédiaire pour éviter d'accéder directement aux données.

### b) **Setter**
```python
@prix.setter
def prix(self, nouveau_prix):
	if nouveau_prix > 0 and isinstance(nouveau_prix, float):
		self._prix = nouveau_prix
	else:
		print("Veuillez entrer un prix valide")
```
Voyons maintenant la syntaxe de la méthode *setter* :

- `@prix.setter` : elle est utilisée pour indiquer qu'il s'agit de la méthode setter pour la propriété *prix*. Notez que nous n'utilisons pas @**property**.setter, mais plutôt @**prix**.setter. Le nom de la propriété est inclus avant ***.setter***.
- `def prix(self, nouveau_prix)` : L'en-tête et la liste des paramètres. Remarquez comment le nom de la propriété est utilisé comme nom du setter. Nous avons également un deuxième paramètre formel (*nouveau\_prix*), qui est la nouvelle valeur qui sera attribuée à l'attribut *prix* (s'il est valide).
- Enfin, nous avons le corps du setter où nous validons l'argument pour vérifier s'il s'agit d'un nombre réel strictement positif. Si l'argument est valide, nous mettons à jour la valeur de l'attribut. Si la valeur n'est pas valide, un message descriptif est imprimé. Vous pouvez choisir comment traiter les valeurs invalides en fonction de vos besoins.

Voici un exemple de l'utilisation de la méthode setter avec @property:

```python
>>> maMaison = Maison(5000000.0)    # Création de l’instance
>>> maMaison.prix = 4500000.0       # Mise a jour de la valeur
>>> maMaison.prix                   # Accès à la valeur
4500000.0
```

Remarquez que nous ne changeons pas la syntaxe, mais que nous utilisons maintenant un intermédiaire (***setter***) pour valider l'argument avant de l'assigner. La nouvelle valeur (4500000.0) est passée comme argument au *setter* :

```python
>>> maMaison.prix = 4500000.0
```

Si nous essayons d'attribuer une valeur non valide, nous voyons le message descriptif. Nous pouvons également vérifier que la valeur n'a pas été mise à jour :
```python
>>> maMaison = Maison(5000000.0)
>>> maMaison.prix = -50
Veuillez entrer un prix valide
>>> maMaison.prix
5000000.0
```
>**Conseil :** Cela prouve que la méthode setter fonctionne comme un intermédiaire. Elle est appelée "en coulisses" lorsque nous essayons de mettre à jour la valeur, alors le message descriptif s'affiche lorsque la valeur n'est pas valide.

### c) **Deleter**

```python
@prix.deleter
def prix(self):
	del self._prix
```
Voyons enfin, la syntaxe de la méthode deleter :

- **@prix.deleter** : elle est utilisée pour indiquer qu'il s'agit de la méthode *deleter* pour la propriété *prix*.  
Remarquez que cette ligne est très similaire à @prix.setter, mais nous définissons maintenant la méthode de suppression, nous écrivons donc **@prix.deleter**.

- **def prix(self)** :  c’est l'en-tête. Cette méthode n'a qu'un seul paramètre formel défini, *self*.
- **del self.\_prix** : c’est le corps qui nous permet de supprimer l'attribut de l'instance.

>Note : remarquez que le nom de la propriété est "réutilisé" pour les trois méthodes

Le code suivant est un exemple d'utilisation de la méthode **deleter** avec **@property**

```python
# Creation de l’instance
>>> maMaison = Maison(5000000.0)

# l’attribut de l’instance existe
>>> maMaison.prix
50000.0

# Suppression de l’attribut 
>>> del maMaison.prix

# l’attribut cesse d’exister
>>> maMaison.prix
Traceback (most recent call last):
  File "<pyshell#35>", line 1, in <module>
    maMaison.prix
  File "<pyshell#20>", line 8, in prix
    return self._prix
AttributeError: 'Maison' object has no attribute '_prix'
```
L'attribut d'instance a été supprimé avec succès. Lorsque nous essayons d'y accéder à nouveau, une erreur est déclenchée car l'attribut n'existe plus.

>**Conseils :** 
Vous ne devez pas nécessairement définir les trois méthodes pour chaque propriété. Vous pouvez définir des propriétés en lecture seule en incluant uniquement une méthode *getter*. Vous pouvez également choisir de définir un *getter* et un *setter* sans *deleter*.
>Si vous pensez qu'un attribut ne doit être défini que lors de la création de l'instance ou qu'il ne doit être modifié qu'en interne dans la classe, vous pouvez omettre le *setter*.
>Vous pouvez choisir les méthodes à inclure en fonction du contexte dans lequel vous travaillez.

## 4.  **Conclusion**

- Vous pouvez définir des propriétés avec la syntaxe *@property*, qui est plus compacte et plus lisible.
- *@property* peut être considéré comme la manière standard de définir des *getters*, *setters* et *deleters*.
- En définissant des propriétés, vous pouvez modifier l'implémentation interne d'une classe sans affecter le programme. Vous pouvez donc ajouter des *getters*, *setters* et *deleters* qui agissent comme des intermédiaires "en coulisses" pour éviter d'accéder ou de modifier les données directement.

J'espère vraiment que cet article vous a plu et qu'il vous a été utile.

#### References :
- Python 101 2nd Edition, Michael Driscoll
- https://www.freecodecamp.org/news/python-property-decorator/