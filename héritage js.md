# L'Héritage en JavaScript - Guide Complet

## 1. Qu'est-ce que l'Héritage ?

### Définition
L'héritage est un mécanisme qui permet à une classe (enfant) de récupérer les propriétés et méthodes d'une autre classe (parent). C'est une relation "EST-UN" (IS-A).

### Schéma conceptuel
```
┌─────────────────┐
│  Classe Parent  │ ← Définit les caractéristiques communes
├─────────────────┤
│ + propriétés    │
│ + méthodes      │
└─────────────────┘
         △
         │ hérite de (IS-A)
         │
┌─────────────────┐
│  Classe Enfant  │ ← Spécialise le comportement
├─────────────────┤
│ + nouvelles     │
│   propriétés    │
│ + nouvelles     │
│   méthodes      │
│ + redéfinition  │
│   de méthodes   │
└─────────────────┘
```

## 2. À Quoi Sert l'Héritage ?

### Objectifs principaux

#### 1. **Réutilisabilité du code**
```
Au lieu de réécrire le même code :

┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│   Chien     │  │    Chat     │  │   Oiseau    │
├─────────────┤  ├─────────────┤  ├─────────────┤
│ nom         │  │ nom         │  │ nom         │
│ âge         │  │ âge         │  │ âge         │
│ manger()    │  │ manger()    │  │ manger()    │
│ dormir()    │  │ dormir()    │  │ dormir()    │
│ aboyer()    │  │ miauler()   │  │ voler()     │
└─────────────┘  └─────────────┘  └─────────────┘
   RÉPÉTITION       RÉPÉTITION      RÉPÉTITION

On factorise avec l'héritage :

        ┌─────────────┐
        │   Animal    │ ← Code commun
        ├─────────────┤
        │ nom         │
        │ âge         │
        │ manger()    │
        │ dormir()    │
        └─────────────┘
               △
       ┌───────┼───────┐
       │       │       │
┌─────────┐ ┌─────┐ ┌─────────┐
│  Chien  │ │Chat │ │ Oiseau  │ ← Spécialisations
├─────────┤ ├─────┤ ├─────────┤
│aboyer() │ │miau │ │voler()  │
└─────────┘ │ler()│ └─────────┘
            └─────┘
```

#### 2. **Organisation hiérarchique**
```
┌─────────────┐
│   Véhicule  │ ← Concept général
├─────────────┤
│ marque      │
│ modèle      │
│ démarrer()  │
│ arrêter()   │
└─────────────┘
       △
   ┌───┴───┐
   │       │
┌─────────┐ ┌─────────────┐
│ Voiture │ │    Moto     │ ← Spécialisations
├─────────┤ ├─────────────┤
│portes   │ │cylindrée    │
│coffre   │ │typeMoteur   │
│klaxon() │ │wheelie()    │
└─────────┘ └─────────────┘
       △           △
   ┌───┴───┐   ┌───┴───┐
   │       │   │       │
┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐
│Sedan│ │ SUV │ │Sport│ │Cross│ ← Spécialisations avancées
└─────┘ └─────┘ └─────┘ └─────┘
```

#### 3. **Polymorphisme**
```javascript
// Un même code peut traiter différents types d'objets
function faireConcertAnimaux(animaux) {
    animaux.forEach(animal => {
        animal.faireDuBruit(); // Chaque animal fait son propre bruit
    });
}

const animaux = [
    new Chien("Rex"),
    new Chat("Mimi"), 
    new Oiseau("Tweety")
];

faireConcertAnimaux(animaux);
// Output: "Wouaf!", "Miaou!", "Cui cui!"
```

## 3. Comment Écrire l'Héritage en JavaScript ?

### Syntaxe ES6 avec `extends` et `super`

#### Exemple 1: Hiérarchie Animal
```javascript
// CLASSE PARENT (SUPERCLASSE)
class Animal {
    constructor(nom, âge) {
        this.nom = nom;
        this.âge = âge;
        console.log(`Animal ${nom} créé`);
    }
    
    // Méthodes communes à tous les animaux
    manger() {
        console.log(`${this.nom} mange`);
    }
    
    dormir() {
        console.log(`${this.nom} dort`);
    }
    
    faireDuBruit() {
        console.log(`${this.nom} fait du bruit`);
    }
    
    sePresenter() {
        console.log(`Je suis ${this.nom}, j'ai ${this.âge} ans`);
    }
}

// CLASSE ENFANT (SOUS-CLASSE)
class Chien extends Animal {
    constructor(nom, âge, race) {
        super(nom, âge); // Appel du constructeur parent
        this.race = race; // Propriété spécifique aux chiens
        console.log(`Chien ${nom} de race ${race} créé`);
    }
    
    // Méthode spécifique aux chiens
    aboyer() {
        console.log(`${this.nom} aboie: Wouaf! Wouaf!`);
    }
    
    // Redéfinition (override) d'une méthode du parent
    faireDuBruit() {
        console.log(`${this.nom} aboie fort!`);
    }
    
    // Extension d'une méthode du parent
    sePresenter() {
        super.sePresenter(); // Appel de la méthode parent
        console.log(`Je suis un ${this.race}`);
    }
}

class Chat extends Animal {
    constructor(nom, âge, couleur) {
        super(nom, âge);
        this.couleur = couleur;
    }
    
    miauler() {
        console.log(`${this.nom} miaule: Miaou!`);
    }
    
    faireDuBruit() {
        console.log(`${this.nom} miaule doucement`);
    }
    
    ronronner() {
        console.log(`${this.nom} ronronne de plaisir`);
    }
}

// UTILISATION
const monChien = new Chien("Rex", 3, "Labrador");
const monChat = new Chat("Mimi", 2, "Blanc");

// Méthodes héritées du parent
monChien.manger();        // "Rex mange"
monChien.dormir();        // "Rex dort"

// Méthodes spécifiques à l'enfant
monChien.aboyer();        // "Rex aboie: Wouaf! Wouaf!"
monChat.miauler();        // "Mimi miaule: Miaou!"

// Méthodes redéfinies
monChien.faireDuBruit();  // "Rex aboie fort!" (version chien)
monChat.faireDuBruit();   // "Mimi miaule doucement" (version chat)

// Méthodes étendues
monChien.sePresenter();   
// "Je suis Rex, j'ai 3 ans"
// "Je suis un Labrador"
```

#### Exemple 2: Hiérarchie Véhicule
```javascript
class Véhicule {
    constructor(marque, modèle, année) {
        this.marque = marque;
        this.modèle = modèle;
        this.année = année;
        this.vitesse = 0;
        this.enMarche = false;
    }
    
    démarrer() {
        this.enMarche = true;
        console.log(`${this.marque} ${this.modèle} démarré`);
    }
    
    arrêter() {
        this.enMarche = false;
        this.vitesse = 0;
        console.log(`${this.marque} ${this.modèle} arrêté`);
    }
    
    accélérer(vitesse) {
        if (this.enMarche) {
            this.vitesse += vitesse;
            console.log(`Vitesse: ${this.vitesse} km/h`);
        }
    }
    
    getInfo() {
        return `${this.marque} ${this.modèle} (${this.année})`;
    }
}

class Voiture extends Véhicule {
    constructor(marque, modèle, année, nombrePortes) {
        super(marque, modèle, année);
        this.nombrePortes = nombrePortes;
        this.coffre = "fermé";
    }
    
    ouvrirCoffre() {
        this.coffre = "ouvert";
        console.log(`Coffre de la ${this.marque} ouvert`);
    }
    
    klaxonner() {
        console.log(`${this.marque} fait: BEEP BEEP!`);
    }
    
    // Redéfinition pour ajouter des infos spécifiques
    getInfo() {
        return `${super.getInfo()} - ${this.nombrePortes} portes`;
    }
}

class Moto extends Véhicule {
    constructor(marque, modèle, année, cylindrée) {
        super(marque, modèle, année);
        this.cylindrée = cylindrée;
        this.casque = false;
    }
    
    mettreCoassure() {
        this.casque = true;
        console.log("Casque mis pour la sécurité");
    }
    
    wheelie() {
        if (this.vitesse > 30) {
            console.log(`${this.marque} fait un wheelie!`);
        }
    }
    
    // Redéfinition du démarrage avec vérification casque
    démarrer() {
        if (!this.casque) {
            console.log("Mettez votre casque avant de démarrer!");
            return;
        }
        super.démarrer(); // Appel de la méthode parent
        console.log("Vroooom! La moto démarre");
    }
}

// UTILISATION
const maVoiture = new Voiture("Renault", "Clio", 2023, 5);
const maMoto = new Moto("Honda", "CBR", 2022, 600);

maVoiture.démarrer();
maVoiture.accélérer(50);
maVoiture.klaxonner();
maVoiture.ouvrirCoffre();

maMoto.démarrer();        // Erreur: pas de casque
maMoto.mettreCoassure();
maMoto.démarrer();        // OK maintenant
maMoto.accélérer(40);
maMoto.wheelie();
```

## 4. Dans Quels Cas Utilise-t-on l'Héritage ?

### ✅ Cas d'usage appropriés

#### 1. **Relation "EST-UN" claire**
```
❌ MAUVAIS: Une Voiture hérite d'un Moteur
   (Une voiture N'EST PAS un moteur, elle EN A un)

✅ BON: Une Voiture hérite d'un Véhicule  
   (Une voiture EST un véhicule)

┌─────────────┐
│  Véhicule   │
└─────────────┘
       △
   ┌───┴───┐
   │       │
┌─────┐ ┌─────┐
│Voitu│ │Moto │ ← EST-UN : ✓
│re   │ │     │
└─────┘ └─────┘
```

#### 2. **Hiérarchie naturelle**
```javascript
// Exemple: Système d'employés
class Employé {
    constructor(nom, salaire) {
        this.nom = nom;
        this.salaire = salaire;
    }
    
    travailler() {
        console.log(`${this.nom} travaille`);
    }
    
    calculerPaie() {
        return this.salaire;
    }
}

class Manager extends Employé {
    constructor(nom, salaire, équipe) {
        super(nom, salaire);
        this.équipe = équipe;
        this.bonus = 0;
    }
    
    gérerÉquipe() {
        console.log(`${this.nom} gère une équipe de ${this.équipe.length} personnes`);
    }
    
    calculerPaie() {
        return super.calculerPaie() + this.bonus;
    }
}

class Développeur extends Employé {
    constructor(nom, salaire, langages) {
        super(nom, salaire);
        this.langages = langages;
        this.projets = [];
    }
    
    coder() {
        console.log(`${this.nom} code en ${this.langages.join(', ')}`);
    }
    
    ajouterProjet(projet) {
        this.projets.push(projet);
    }
}
```

#### 3. **Spécialisation progressive**
```
┌─────────────┐
│   Forme     │ ← Concept abstrait
├─────────────┤
│ couleur     │
│ dessiner()  │
└─────────────┘
       △
   ┌───┼───┐
   │   │   │
┌─────┐│┌─────┐
│Rect │││Cercle│ ← Formes géométriques
│angle│││     │
└─────┘│└─────┘
       │
   ┌───┴───┐
   │       │
┌─────┐ ┌─────┐
│Carré│ │Losang│ ← Spécialisations du rectangle
└─────┘ │ge   │
        └─────┘
```

### ❌ Cas où éviter l'héritage

#### 1. **Relation "A-UN" (composition préférée)**
```javascript
// ❌ MAUVAIS: Héritage pour "avoir"
class Voiture extends Moteur {  // Une voiture N'EST PAS un moteur
    // ...
}

// ✅ BON: Composition
class Voiture {
    constructor() {
        this.moteur = new Moteur(); // Une voiture A un moteur
    }
}
```

#### 2. **Hiérarchie trop profonde**
```
❌ ÉVITER: Hiérarchie trop complexe
Être vivant → Animal → Mammifère → Félin → Chat domestique → Chat siamois → Chat siamois aux yeux bleus
(7 niveaux = difficile à maintenir)

✅ PRÉFÉRER: Hiérarchie simple
Animal → Chat (avec propriétés: race, couleurYeux, etc.)
```

## 5. Relations Parent ↔ Enfant

### Schéma des relations
```
         PARENT
    ┌─────────────┐
    │   Animal    │
    ├─────────────┤
    │ nom         │ ←─── Hérité par tous les enfants
    │ âge         │
    │ manger()    │ ←─── Méthode commune
    │ dormir()    │
    └─────────────┘
           △
           │ extends
           │
         ENFANT  
    ┌─────────────┐
    │   Chien     │
    ├─────────────┤
    │ race        │ ←─── Propriété spécifique
    │ aboyer()    │ ←─── Méthode spécifique
    │ faireBruit()│ ←─── Redéfinition de méthode parent
    └─────────────┘

FLÈCHES DE COMMUNICATION:
Parent ──▶ Enfant : Transmission (héritage)
Enfant ──▶ Parent : Appel (super)
```

### Communication Parent → Enfant

#### 1. **Héritage automatique**
```javascript
class Parent {
    propriétéParent = "Je viens du parent";
    
    méthodeParent() {
        console.log("Méthode du parent appelée");
    }
}

class Enfant extends Parent {
    // Hérite automatiquement de propriétéParent et méthodeParent
    
    tester() {
        console.log(this.propriétéParent); // Accès direct
        this.méthodeParent();              // Appel direct
    }
}

const enfant = new Enfant();
enfant.tester();
// "Je viens du parent"
// "Méthode du parent appelée"
```

### Communication Enfant → Parent

#### 1. **Appel du constructeur parent avec `super()`**
```javascript
class Véhicule {
    constructor(marque, vitesseMax) {
        this.marque = marque;
        this.vitesseMax = vitesseMax;
        console.log(`Véhicule ${marque} initialisé`);
    }
}

class Voiture extends Véhicule {
    constructor(marque, vitesseMax, nombrePortes) {
        // OBLIGATOIRE: appeler super() avant d'utiliser 'this'
        super(marque, vitesseMax); // ←─── Communication avec parent
        
        this.nombrePortes = nombrePortes;
        console.log(`Voiture avec ${nombrePortes} portes créée`);
    }
}
```

#### 2. **Appel de méthodes parent avec `super.méthode()`**
```javascript
class Animal {
    faireDuBruit() {
        console.log("L'animal fait du bruit");
    }
    
    sePresenter() {
        console.log(`Je suis un animal`);
    }
}

class Chien extends Animal {
    faireDuBruit() {
        // Extension de la méthode parent
        super.faireDuBruit(); // ←─── Appel explicite au parent
        console.log("Plus spécifiquement: Wouaf!");
    }
    
    sePresenter() {
        super.sePresenter(); // ←─── Réutilise le code parent
        console.log("Et je suis un chien");
    }
}

const chien = new Chien();
chien.faireDuBruit();
// "L'animal fait du bruit"
// "Plus spécifiquement: Wouaf!"

chien.sePresenter();
// "Je suis un animal"
// "Et je suis un chien"
```

### Exemple complet: Communication bidirectionnelle
```javascript
class Personne {
    constructor(nom, âge) {
        this.nom = nom;
        this.âge = âge;
        console.log(`Personne ${nom} créée`);
    }
    
    sePresenter() {
        return `Je suis ${this.nom}, ${this.âge} ans`;
    }
    
    travailler() {
        return `${this.nom} travaille`;
    }
}

class Étudiant extends Personne {
    constructor(nom, âge, université) {
        super(nom, âge); // ←─── Enfant → Parent
        this.université = université;
        this.notes = [];
    }
    
    // Redéfinition avec extension
    sePresenter() {
        const baseInfo = super.sePresenter(); // ←─── Enfant → Parent
        return `${baseInfo} et j'étudie à ${this.université}`;
    }
    
    // Redéfinition complète
    travailler() {
        return `${this.nom} étudie dur à ${this.université}`;
    }
    
    ajouterNote(note) {
        this.notes.push(note);
    }
    
    getMoyenne() {
        return this.notes.reduce((sum, note) => sum + note, 0) / this.notes.length;
    }
}

// Utilisation montrant les deux directions
const étudiant = new Étudiant("Alice", 20, "Sorbonne");

// Parent → Enfant : Alice hérite de nom, âge
console.log(étudiant.nom); // "Alice" (hérité du parent)

// Enfant → Parent : Alice utilise la méthode parent étendue
console.log(étudiant.sePresenter()); 
// "Je suis Alice, 20 ans et j'étudie à Sorbonne"

// Méthode spécifique à l'enfant
étudiant.ajouterNote(15);
étudiant.ajouterNote(18);
console.log(`Moyenne: ${étudiant.getMoyenne()}`); // "Moyenne: 16.5"
```

## Résumé Visuel

```
HÉRITAGE = RELATION "EST-UN"

┌─────────────────────────────────────────────────────────┐
│                    CLASSE PARENT                        │
│  ┌─────────────────────────────────────────────────┐    │
│  │ • Propriétés communes                           │    │
│  │ • Méthodes communes                             │    │
│  │ • Comportements généraux                        │    │
│  └─────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────┘
                            │
                            ▼ extends
┌─────────────────────────────────────────────────────────┐
│                   CLASSE ENFANT                        │
│  ┌─────────────────────────────────────────────────┐    │
│  │ • HÉRITE: Propriétés + méthodes du parent       │    │
│  │ • AJOUTE: Nouvelles propriétés/méthodes         │    │
│  │ • REDÉFINIT: Spécialise certaines méthodes      │    │
│  │ • ÉTEND: Améliore les méthodes existantes       │    │
│  └─────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────┘

COMMUNICATION:
Parent ──transmission──▶ Enfant (héritage automatique)
Enfant ──super()──────▶ Parent (appel explicite)
```