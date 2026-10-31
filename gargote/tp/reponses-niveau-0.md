## 0.2 Lire le schéma relationnel
### 1.
- categorie(<u>id</u>, libelle)
- plat (<u>id</u>, nom, prix, #id_categorie)
- commande (<u>id<u/>, date_commande, numero_table)
- commande_ligne (<u>id<u/>, #id_commande, #id_plat, quantite, prix_unitaire)


### 2.
Parce que il est possible que le prix est different du prix du plat quand on a fait l achat.
Exemple : le prix du plat Mi sao est de 6.500ar, si on le change par exemple à 7.000ar, le système dira que tous les Mi sao vendu sont à 7.000ar, alors que c faux, dans les anciens commandes, le Mi sao etait à 6.500ar


### 3.
Quand on supprime une commande, ces lignes de commandes sont aussi supprimées
L'instruction responsable: **REFERENCES commande(id) ON DELETE CASCADE** dans la table commande_ligne

### 4. 
- On peut insérer des prix negatives
- On peut insérer des quantites negatives
- On peut inséer des numero de table negatives

## 0.3 Interroger l’API à la main

### 5.
Pour la requete b on obtient 201 pas 200 car 201 est la reponse conventionelle pour POST

### 6.
C'est Postgres qui bloque en cause du foreign key id_table present dans commande_ligne

```
try {
    $this->plats->delete($id);
} catch (\Throwable $e) {
    return $this->erreur(
        'Ce plat figure déjà dans des commandes, il ne peut pas être supprimé.',
        409
    );
}
```

### 7.
```
curl -i -X POST <http://localhost:8080/api/commandes> \
-H "Content-Type: application/json" \
-d '{"numero_table":6,"lignes":[{"id_plat":4,"quantite":2},{"id_plat":10,"quantite":1}]}'
```

## 0.4 Suivre une requête de bout en bout

### 8.
Le code metier se trouve dans backend/app/Services/CommandeService.php, dans la methode creer()
Il n'est pas dans le controlleur car ce dernier ne fait que gerer les requetes et reponses HTTP


## 0.5 Repérer les notions de Vue

### 9.
Ce choix est du pour respecter le principe de conception Mere - Fille , view dit juste au parents de modifier la quantite mais c'est CommandeView qui est la mere qui possede reellement les donneees 

### 10.
Watch nous permet de copoier les informations d'un plat a modifier dans les champs du formulaire de modification

## 0.6 Premières modifications

### a
Nom change en gargote tsy matsiro

### b
INSERT INTO categorie (libelle) VALUES ('Goûter');

### 11
La route GET/api/categories appelle Categories::index() qui lit et prend les categories dans la BDD, CarteView.vue appelle la fonction lister pour actualiser dynamiquement la liste des categories que Vue va reprendre pour actualiser sa liste deroulante
