# Sujet de TP Angular 

![](https://github.com/barais/teaching-jxs-angular/raw/master/img/banner.png)

Votre aventure dans le monde merveilleux des frameworks JavaScript commence avec Angular.

Angular est un framework développé par Google. Ce framework se base principalement sur la définition de composants et de services. L'objectif est d'avoir un code modulaire et réutilisable. Le langage recommandé pour programmer avec Angular et celui que nous allons utiliser est Typescript. On peut aussi utiliser Javascript 5/6 ou bien Dart.

Pour vous aider dans votre quête, le professeur Chen vous a laissé des instructions ainsi que les
éléments de bases pour créer un Pokédex à cette adresse : [https://github.com/barais/teaching-jxs-angular](https://github.com/barais/teaching-jxs-angular) (l'utilisation d'une bicyclette est conseillée pour s'y rendre plus vite).


C'est un [clone](https://github.com/gbecan/teaching-jxs-angular4) du sujet développé par Guillaume Becan (ancien doctorant de l'équipe). 

![](https://github.com/barais/teaching-jxs-angular/raw/master/img/fig1.png)

## Step 0

Vérifiez l'installation de nodejs à l'aide de nvm (voir [guide d'instal](https://olivier.barais.fr/teaching/2022-09-08-Operation_portable_M2_ISTIC#etape-4-nodejs))

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
# Relancer bash
bash
nvm install 22
```

### Installez ng-cli

```bash
npm install -g @angular/cli
```

## Step 1: Initialisation du projet

```bash
# génération d'un nouveau projet
ng new pokedemo --standalone=false --routing=true --ssr=false
cd pokedemo
```

```bash
# lancement du serveur
ng serve
```

Observez bien le squelette du projet généré. Il vient avec un point d'entrée (main.ts) et le fichier index.html racine. Ce dernier charge un composant sous la directive (selector) *&lt;app-root>&lt;/app-root>*. Cette directive demande l'instantiation d'un composant *app* défini dans le répertoire *app*. Ce composant est défini par une classe *app.component.ts*, un template *app.html*, une classe de test *app.spec.ts*et un fichier de style *app.css*. 

Changez le template *app.html* en remplaçant le code du template par  les lignes suivantes. Vous constaterez que l'application est rechargée automatiquement. 

```html
<div style="text-align:center">
  <h1>
   {{ title() }}!
  </h1>
</div>
```


## Step 2: Recherche d'un pokemon via son numéro

La première étape pour développer notre pokédex consiste à proposer au dresseur de rechercher un
pokemon via son numéro. Nous allons donc créer un composant Angular qui va contenir et gérer cette
interface. Pour créer un composant, il suffit de créer une classe et de lui ajouter l'annotation *@Component*. Cette annotation peut être complétée par plusieurs paramètres. Par exemple:

- **selector** permet de définir le nom de l'élément html qui sera remplacé par notre composant
- **templateUrl** permet de spécifier le fichier html qui servira de vue au composant
- **styleUrls** permet de spécifier les différentes feuilles de style CSS appliquées au composant

```ts
@Component({
selector: "my-component",
templateUrl: "./my-component.html",
styleUrls: ["./my-component.css"],
})
export class MyComponent {
}
```

Pour faciliter la création d’un composant, nous allons utiliser l’outil [angular-cli](https://github.com/angular/angular-cli/blob/master/packages/angular/cli/README.md). Cet outil permet en
particulier de générer facilement les différents concepts liés à Angular comme les composants, les
services ou les directives. Par exemple, pour générer le composant précédent, il suffit de taper 

```bash
ng generate component my-component
```

Vous constaterez que ng-cli génère automatiquement une structure pour vous avec un répertoire par composant. 

### Q1 : 

Créer un composant avec un élément *&lt;input>* afin de récupérer l'id recherché. Ajouter votre
composant à la liste des directives et au template du composant *my-component*.

Charger ce composant en ajoutant le selector de ce composant dans le template du composant *app.component.html*

```html
<app-my-component></app-my-component>
```

Vous constaterez que la page web contient maintenant le code html résultat du composant *my-component* à l'intérieur du composant *app.component*. 



Nous allons maintenant utiliser le data-binding d'Angular pour lier l'élément *&lt;input>* à un attribut de
notre composant. En quelque sorte, nous allons lier la vue à notre modèle. 

Pour cela il nous faut charger un module supplémentaire. Dans app.module.ts ajoutez un import

```ts
import { FormsModule } from '@angular/forms';
```

et dans la section import, ajoutez le chargement de ce module pour votre application

```ts
  imports: [
    FormsModule, //Line to add
    BrowserModule
  ],
```


Pour ajoutez le databinding, on utilise la directive  ngModel qui s'utilise comme ceci dans le fichier my-component-component.html :

```html
<input [(ngModel)]="id">
```

Ce code lie la valeur de l'élément input à l'attribut *id* de notre composant.


Il faut aussi aussi ajouter l'attribut dans la classe métier du composant.  Dans  my-component-component.ts, ajouter l'attribut id de type string

```ts
  id: string = '';
```


### Q2 : 

Créer un attribut id et lier le à l'élément *&lt;input>* précédemment créé.

Pour tester le lien entre l'attribut et l'élément HTML, nous allons utiliser une autre syntaxe utilisant
aussi le mécanisme de data-binding : *{{myAttribute}}*. Cette syntaxe permet d'afficher la valeur d'un
attribut du composant sur la page.


Ajoutez {{id}} quelque part au sein de cotre template de votre composant (my-component-component.html ). 

Vous constaterez que dès que l'input est modifié par l'utilisateur, la vue contenant la valeur de l'id est elle aussi modifiée. 

### Q3 : 

Créer un deuxième champs input en mode readonly et lié les deux par un id.  Afficher la valeur de l'id renseigné dans la balise *&lt;input>* venant d'être insérée.


### Q3bis :

Expliquer pourquoi il devient difficile de faire une attaque XSS sur une application angular

[lien](https://vitalflux.com/angular-prevent-xss-attacks-code-examples/)


## Recherche dans une liste


Malheureusement, seul le professeur Chen connaît précisément le numéro de tous les pokemon. Pour
aider les jeunes dresseurs à utiliser le pokédex, nous allons offrir la liste des pokemon ainsi qu'un
champ de recherche pour filtrer cette liste.

### Q4 : 

Créer une classe Pokemon qui comporte un id et un nom. Nous compléterons la classe au fur et à
mesure du TP.

```bash
ng g class pokemon
```
### Q5 : 

Créer une liste fictive (4-5 éléments suffiront) de pokemon dans le composant précédemment
créé.

### Q6 : 

Afficher la liste des pokemon dans [https://angular.io/guide/template-syntax#ngforof](https://angular.io/guide/template-syntax#ngforof)
une balise *&lt;select>* en utilisant \*ngFor

### Q7 : 

Récupérer le choix du dresseur en liant la balise *&lt;select>* au modèle avec ngModel.


### Q8 : 

Comme la liste des pokemon peut être très longue, nous allons proposer au dresseur de filtrer la liste.


Ajouter un champ de texte et récupérer sa valeur dans un attribut. Nous allons devoir créer un filtre à l'aide d'un *pipe* Angular. 

Tout d'abord créons un *pipe*. 

```bash
ng g pipe filter-pokemon--pipe
```

dans la classe générée pour le pipe, vous verrez ce filtre
prend deux paramètres : le nom de l’attribut à filtrer et la valeur à rechercher. La fonction *transform* ressemblera à cela. 

```ts
 transform(pokes: any[], property?: string, searchString?: string): any {
    if(typeof searchString == 'undefined'){
      return pokes;
    }
    else if (typeof pokes !== 'undefined' && typeof property !== 'undefined') {
      return pokes.filter((poke) => {
        return poke[property].toLowerCase().indexOf(searchString.toLowerCase()) !== -1;
      });
    } else {
      return [];
    }
  }
  ```

En partant de la page de [documentation](https://angular.io/guide/pipes) sur les *pipes*, faites en sorte de permettre le filtre de votre liste depuis une entrée utilisateur. 


### Q9 : 

Pour valider le choix du dresseur, nous allons ajouter un bouton « Go ! » dont le comportement sera défini dans notre composant. Pour lier un évènement à notre composant, on utilise la syntaxe qui permet d'appeler une méthode de notre composant. 

```html 
(eventName)="codeToExecute()"
```



Ajouter un *&lt;button>* à la page et lier l'évènement click à une méthode du contrôleur. Pour le
moment la méthode se contentra d'afficher l'id ou le nom du pokemon recherché dans la console.


## Intégration de composants existants


### Q9 bis: 


Intégration de composants material UI. 

En suivant ce [guide](https://material.angular.io/guide/getting-started), intgréer un composant angular à votre application. 
Regardez le nombre de module util à la comilation et le nombre de dépendances utilisés. (Dans votre node_module)



## Accès à une API

Le site [http://pokeapi.co/](http://pokeapi.co/) propose une API contenant de nombreuses informations sur les pokemon. En
particulier, l'API offre la liste des pokemon (api/v2/pokedex/1) ainsi que des informations détaillées
pour chacun d'entre eux (api/v2/pokemon/54 ou api/v2/pokemon/psyduck). Nous allons utiliser cette
API comme source d'information pour notre pokédex.

Angular fournit un [service HTTP](https://angular.io/guide/http) qui va nous permettre de communiquer avec PokéAPI. Angular
utilise l'injection de dépendances pour fournir les services. Cela permet en particulier d'instancier un
service qu'une seule fois pour toute l'application ou bien une partie de celle-ci. Pour encapsuler l'accès à
l'API, nous allons nous même créer un service. Un service Angular est en fait une classe.

### Q10 : 

Créer un service pour gérer l'accès à PokéAPI.

```bash
ng generate service
```

Ajouter un paramètre à son constructeur afin de récupérer le service http. Ajouter aussi l'annotation *@Injectable()* afin de
spécifier à Angular que votre service comporte une dépendance vers un autre service.

### Q11 : 

Créer une méthode pour récupérer la liste des pokemon en utilisant le service *http*.

### Q12 : 

Utiliser ce service dans le composant de recherche de pokemon pour remplacer la liste fictive de pokémons. Pour cela, ajouter le paramètre providers au module AppModule comme ceci :


```ts
providers: [PokeApiService] 
```

Ainsi Angular saura quelle classe injecter dans vos composants.

### Q13 : 

Créer une autre méthode pour récupérer les informations sur un pokémon. Compléter la classe
Pokémon afin de recueillir ces informations.

### Q14 : 

Créer un nouveau composant dédié à l'affichage des informations d'un pokémon. Utiliser le
service précédemment créé pour récupérer les informations d'un pokémon. Combiner les différents
mécanismes de data-binding vus jusqu'ici pour afficher l'id, le nom et les statistiques d'un pokémon.



## Communication entre contrôleurs



À présent, nous avons deux parties à notre application. La première permet de rechercher un pokémon
grâce à son numéro ou son nom. La deuxième récupère et affiche les informations d'un pokémon. Il ne
reste plus qu'à relier ces deux parties pour finaliser notre pokédex. Pour faire communiquer nos deux
composants, nous allons créer un nouveau service qui va contenir les informations à partager. Comme
pour le service dédié à l'API, ce service ne sera instancié qu'une seule fois et permettra donc l'échange
d'informations.


### Q15 : 

Créer un service contenant l'id du pokémon recherché. Injecter ce service dans le composant de
recherche et le composant d'affichage des informations. On n'oublira pas de l'ajouter dans les providers
du module AppModule.
Les deux composants ont maintenant un service pour partager des informations liées et utilisent les
mêmes informations. Cependant, les informations du pokémon ne sont pas mises à jour si le dresseur
change le numéro ou le nom du pokémon recherché. Pour détecter les changements de ces deux
attributs, nous allons utiliser la notion d'observable.

### Q16 : 

Créer un observable dans le service précédemment créé. Le composant d'affichage d'informations
d'un pokémon peut maintenant souscrire à cet observable pour détecter le changement de pokémon.

## Intégration de composants open source

### Q17: 
La richesse d'Angular réside également dans son écosystème de librairies de composants réutilisables et open source.

* Vous allez maintenant intégrer un composant Angular issue d'une librairie open source dans votre application. 

  Dans le cadre de ce tp je vous recommande d'utiliser [primeng](https://primefaces.org/primeng/showcase/#/setup) 
  ou [Angular Material](https://material.angular.io/guide/getting-started).

  Vous trouverez également d'autres librairies de composants intéressentes sur la page [Angular Dev Resources](https://angular.io/resources?category=development) du site d'Angular.


* Regardez le nombre de module utile à la compilation et le nombre de dépendances utilisés. (Dans votre node_module)
