﻿
![logo.png](./imgs/logo.png)  

# ECF3 - Gestion des Étudiants, des cours et des notes  
Ce projet est une application Java développée en utilisant Spring Boot. Il suit le modèle n-tiers avec une séparation entre la présentation, la logique métier et l'accès aux données. L'application permet de gérer des étudiants, leurs notes et leurs cours.

## Architecture du projet

### Couches de l'application
- **Couche de présentation (Controller)** : Fournit les points d'entrée pour les requêtes HTTP et les expose via des API REST.
- **Couche métier (Service)** : Contient la logique métier et appelle les services pour gérer les entités `Étudiant`, `Cours` et `Note`.
- **Couche d'accès aux données (DAO)** : Utilise JPA/Hibernate pour interagir avec la base de données, effectue des opérations CRUD sur les entités.

### Technologies utilisées
- **Spring Boot** pour le développement de l'application.
- **Spring Data JPA** pour l'accès aux données et la gestion des entités.
- **H2 Database** pour la base de données en mémoire.
- **JUnit et Mockito** pour les tests unitaires.
- **Maven** pour la gestion des dépendances et la construction du projet.

## Installation et configuration

1. Clonez le projet depuis le dépôt GitHub :
    ```bash
    git clone https://github.com/sialorama/ECF3-Gestion-notes-etudiants-api.git
    ```

2. Naviguez dans le dossier du projet :
    ```bash
    cd ECF3-Gestion-notes-etudiants-api
    ```

3. Compilez et exécutez l'application avec Maven :
    ```bash
    mvn clean spring-boot:run
    ```

4. L'application sera disponible à l'adresse suivante :
    ```
    http://localhost:8080
    ```
## Utiliser Postman

### La collection des différents endpoints est accessible ici
 ```
https://elements.getpostman.com/redirect?entityId=36416744-0f36ec23-f80a-4a6f-aa68-3c171d873262&entityType=collection
 ```
### Quelques opérations CRUD dans postman

### 1. Gestion des Étudiants

- **Ajouter un étudiant**
    - **URL** : `/etudiants`
    - **Méthode HTTP** : `POST`
    - **Corps de la requête (JSON)** :
    ```json
    {
        "id": 1,
        "nom": "Maxou",
        "prenom": "Le Gall"
    }
    ```
    - **Réponse** : Retourne l'étudiant ajouté.

- **Consulter un étudiant**
    - **URL** : `/etudiants/{id}`
    - **Méthode HTTP** : `GET`
    - **Réponse** : Retourne les détails de l'étudiant spécifié par l'ID.

- **Consulter les notes d'un étudiant**
    - **URL** : `/etudiants/{id}/notes`
    - **Méthode HTTP** : `GET`
    - **Réponse** : Retourne la liste des notes de l'étudiant spécifié par l'ID.

### 2. Gestion des Notes

- **Ajouter une note**
    - **URL** : `/notes`
    - **Méthode HTTP** : `POST`
    - **Corps de la requête (JSON)** :
    ```json
    {
        "cours": {
            "id": 1,
            "nom": "Mathématiques"
        },
        "valeur": 15.5,
        "etudiant": {
            "id": 1
        }
    }
    ```
    - **Réponse** : Retourne la note ajoutée.

- **Supprimer une note**
    - **URL** : `/notes/{id}`
    - **Méthode HTTP** : `DELETE`
    - **Réponse** : Confirme la suppression de la note spécifiée par l'ID.

## Tests

### Tests unitaires
Les tests unitaires sont implémentés avec JUnit et Mockito pour tester la couche service. Ils permettent de vérifier la bonne gestion des opérations telles que l'ajout d'un étudiant ou la consultation de ses notes.

Exemple de test pour l'ajout d'un étudiant :
```java

import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;

class EtudiantServiceTest {

    @Mock
    private EtudiantDAO etudiantDAO;

    @InjectMocks
    private EtudiantService etudiantService;

    @BeforeEach
    void setUp() {
        MockitoAnnotations.openMocks(this);
    }

    @Test
    void ajouterEtudiant_ShouldAddEtudiant() {
        Etudiant etudiant = new Etudiant();
        when(etudiantDAO.save(etudiant)).thenReturn(etudiant);

        Etudiant result = etudiantService.ajouterEtudiant(etudiant);

        assertEquals(etudiant, result);
        verify(etudiantDAO, times(1)).save(etudiant);
    }

