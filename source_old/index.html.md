---
title: Documentation API CRDM UNIVERSE

language_tabs: # must be one of https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers

toc_footers:
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the CRDM UNIVERSE API
---

# Introduction

Documentation pout l'API de l'application CRDM UNIVERSE


# Authentification


CRDM SYSTEM utilise un couple login/password personnel pour autoriser l' acces a son API.

C'est une authentification de type Cookie

<aside class="info">
Il faut envoyer le cookie XSRF-TOKEN dans le header de la requete pour pouvoir acceder aux formulaires.
</aside>

[Lien vers le detail du XSRF-TOKEN](#xsrf-token)


## Login

Ce endpoint permet de s'identifier.

### HTTP Request

`POST http://example.com/login`

### Parametres POST 

| Parametre       | Description            |
|-----------------|------------------------|
| email (text)    | adresse mail           |
| password (text) | mot de passe           |
| remember (bool) | case se souvenir de moi |

### Code HTTP retourne

`204 No Content             : Login OK`

`422 Unprocessable Content  : Echec de la validation des donnees (voir message particuler)`

```json
{
    "message": "The given data was invalid.",
    "errors": {
        "email": [
            "The email field is required."
        ],
        "password": [
            "The password field is required."
        ],
        "remember": [
            "The remember field is required."
        ]
    }
}
```

`401 Unauthorized          : Login KO`
## Logout

Ce endpoint permet de se deconnecter.

### HTTP Request

`POST http://example.com/logout`

### Parametres POST 

| Parametre | Description |
|-----------|-------------|
| none      | rien        |


### Code HTTP retourne

`204 No Content : Logout OK`

`401 Unauthorized : Logout KO`

## XSRF-TOKEN

Ce endpoint permet de se recuperer le cookie XSRF-TOKEN.

### HTTP Request

`GET http://example.com/sanctum/csrf-cookie`

### Parametres GET 

| Parametre | Description |
|-----------|-------------|
| none      | rien        |


### Code HTTP retourne

`204 No Content : Cookie bien recupere OK`

`401 Unauthorized : Probleme recuperation Cookie KO`

# Utilisateurs

## Lister les utilisateurs

> Cet appel retourne un fichier JSON structure tel que ci-dessous:

```json
[
  {
        "id": 1,
        "name": "Lamouliatte",
        "first_name": "Nicolas",
        "title": "M.",
        "phone_number": "+33752639531",
        "email": "nicolas@crdm-developpements.com",
        "email_verified_at": "2023-09-21T14:48:30.000000Z",
        "deleted_at": null,
        "created_at": "2023-09-21T14:48:30.000000Z",
        "updated_at": "2023-09-21T14:48:30.000000Z",
        "preferences": {
            "localisation": "fr",
            "isDarkModeEnabled": false
        }
    },
    {
        "id": 2,
        "name": "Riotte",
        "first_name": "Christophe",
        "title": "M.",
        "phone_number": "+33782170073",
        "email": "christophe.riotte@crdm-developpements.com",
        "email_verified_at": "2023-09-21T14:48:30.000000Z",
        "deleted_at": null,
        "created_at": "2023-09-21T14:48:30.000000Z",
        "updated_at": "2023-09-21T14:48:30.000000Z",
        "preferences": {
            "localisation": "fr",
            "isDarkModeEnabled": false
        }
    }
]
```

Ce endpoint donne la liste des utilisateurs (non supprimes).

### HTTP Request

`GET http://example.com/api/users`

### Parametres URL 

| Parametre | Description |
|-----------|-------------|
| none      | rien        |

### Code HTTP retourne

`200 Ok : Pas de probleme`

`401 Unauthorized : Login a faire`

<!--

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

-->

## Detail d'un utilisateur particulier

> Cet appel retourne un fichier JSON structure tel que ci-dessous:

```json
[
    {
        "id": 1,
        "name": "Lamouliatte",
        "first_name": "Nicolas",
        "title": "M.",
        "phone_number": "+33752639531",
        "email": "nicolas@crdm-developpements.com",
        "email_verified_at": "2023-09-21T14:48:30.000000Z",
        "deleted_at": null,
        "created_at": "2023-09-21T14:48:30.000000Z",
        "updated_at": "2023-09-21T14:48:30.000000Z",
        "preferences": {
            "localisation": "fr",
            "isDarkModeEnabled": false
        }
    }
]
```

Ce endpoint retourne un utilisateur.


### HTTP Request

`GET http://example.com/api/user/<ID>`

### Parametres URL 

| Parametre | Description               |
|-----------|---------------------------|
| ID  (int) | L' ID du user a consulter |

### Code HTTP retourne

`200 Ok : Pas de probleme`

`401 Unauthorized : Login a faire`

`404 Not Found : Utilisateur non trouve`


## Detail de l'utilisateur connecte

Ce endpoint retourne l' utilisateur connecte.


### HTTP Request

`GET http://example.com/api/user/`

### JSON Example

[Le meme que pour le detail d'un utilisateur particulier](#detail-d-39-un-utilisateur-particulier)

### Code HTTP retourne

`200 Ok : Pas de probleme`

`401 Unauthorized : Login a faire`

## Creation d'un l'utilisateur

Ce endpoint permet de creer un utilisateur.
Les champs attendus sont les suivants:

- name
- first_name
- title
- phone_number
- email
- localisation

<aside class="info">
Pas de mot de passe, le user est cree avec un mot de passe aleatoire, il devra le changer via le lien perte de mot de passe</aside>

Ce endpoint envoi un email au nouvel utilisateur (bcc le user qui fait la creation) pour le renvoyer vers la page reset password.


### HTTP Request

`POST http://example.com/api/user/<ID>`

### JSON Example

[Le meme que pour le detail d'un utilisateur particulier](#detail-d-39-un-utilisateur-particulier)

### Parametres POST 

| Parametre                                                | Description                                |
|----------------------------------------------------------|--------------------------------------------|
| name (string 100)                                        | le nom de l'utilisateur                    |
| first_name (string 100)                                  | le prenom de l'utilisateur                 |
| title  (string 10)                                       | le titre (civilite) de l'utilisateur       |
| phone_number (string 20)                                 | le numero de telephone de l'utilisateur    |
| email (string 255)                                       | l'adresse email de l'utilisateur           |
| localisation (string 2) optionnel ('fr' ou 'en' ou 'es' ou 'de') | la localisation de l'utilisateur pour I18N |


### Code HTTP retourne

`204 No Content : Pas de probleme`

`401 Unauthorized : Login a faire`

`422 Unprocessable Content  : Echec de la validation des donnees`

## Suppression d'un l'utilisateur

Ce endpoint permet de supprimer un utilisateur.

### HTTP Request

`DELETE http://example.com/api/user/<ID>`

### Parametres URL

- ID (int) : L' ID du user a supprimer


### Code HTTP retourne

`204 No Content : Pas de probleme`

`401 Unauthorized : Login a faire`

`404 Not Found : Utilisateur non trouve`

`422 Unprocessable Content  : Echec de la validation des donnees`




## Modification d'un l'utilisateur particulier

Ce endpoint permet de modifier un utilisateur specifique.
Les champs attendus sont les suivants:

- name
- first_name
- title
- phone_number

Ce endpoint retourne un utilisateur.


### HTTP Request

`PATCH http://example.com/api/user/<ID>`

### Parametres URL

- ID (int) : L' ID du user a modifier

### JSON Example

[Le meme que pour le detail d'un utilisateur particulier](#detail-d-39-un-utilisateur-particulier)



### Parametres PATCH 

| Parametre                | Description                             |
|--------------------------|-----------------------------------------|
| name (string 100)        | le nom de l'utilisateur                 |
| first_name (string 100)  | le prenom de l'utilisateur              |
| title  (string 10)       | le titre (civilite) de l'utilisateur    |
| phone_number (string 20) | le numero de telephone de l'utilisateur |


### Code HTTP retourne

`200 Ok : Pas de probleme`

`401 Unauthorized : Login a faire`

`404 Not Found : Utilisateur non trouve`

`422 Unprocessable Content  : Echec de la validation des donnees`


## Modification de l'utilisateur connecte

Ce endpoint permet de modifier l'utilisateur connecte.
Les champs attendus sont les suivants:

- name
- first_name
- title
- phone_number

Ce endpoint retourne un utilisateur.


### HTTP Request

`PATCH http://example.com/api/user/`

### JSON Example

[Le meme que pour le detail d'un utilisateur particulier](#detail-d-39-un-utilisateur-particulier)

### Parametres PATCH 

| Parametre                | Description                             |
|--------------------------|-----------------------------------------|
| name (string 100)        | le nom de l'utilisateur                 |
| first_name (string 100)  | le prenom de l'utilisateur              |
| title  (string 10)       | le titre (civilite) de l'utilisateur    |
| phone_number (string 20) | le numero de telephone de l'utilisateur |


### Code HTTP retourne

`200 Ok : Pas de probleme`

`401 Unauthorized : Login a faire`

`422 Unprocessable Content  : Echec de la validation des donnees`

## Modification du mot de passe de l'utilisateur connecte

Ce endpoint permet de modifier le mot de passe de l'utilisateur connecte.
Les champs attendus sont les suivants:

- password

Le nouveau mot de passe ne doit pas etre identique au mot de passe actuel.

Conditions de validation du mot de passe:

- 8 caracteres minimum
- 1 lettre minuscule minimum
- 1 lettre majuscule minimum
- 1 chiffre minimum
- 1 caractere special minimum

Ce endpoint retourne un utilisateur.


### HTTP Request

`PATCH http://example.com/api/user/settings/password`

### JSON Example

[Le meme que pour le detail d'un utilisateur particulier](#detail-d-39-un-utilisateur-particulier)

### Parametres PATCH 

| Parametre                              | Description                             |
|----------------------------------------|-----------------------------------------|
| old_password (string 255)              | l'ancien mot de passe de l'utilisateur |
| new_password (string 255)              | le nouveau mot de passe de l'utilisateur |
| new_password_confirmation (string 255) | la confirmation du nouveau mot de passe de l'utilisateur |

### Code HTTP retourne

`200 Ok : Pas de probleme`

`401 Unauthorized : Login a faire`

`422 Unprocessable Content  : Echec de la validation des donnees`


## Modification de la preference darkMode de l'utilisateur connecte

Ce endpoint permet de modifier la preference darkMode de l'utilisateur connecte.
Les champs attendus sont les suivants:

- state

Ce endpoint retourne un utilisateur.


### HTTP Request

`PATCH http://example.com/api/user/preferences/darkMode`

### JSON Example

[Le meme que pour le detail d'un utilisateur particulier](#detail-d-39-un-utilisateur-particulier)

### Parametres PATCH 

| Parametre                 | Description               |
|---------------------------|---------------------------|
| state (bool)              | Etat activation dark mode |


### Code HTTP retourne

`200 Ok : Pas de probleme`

`401 Unauthorized : Login a faire`

`422 Unprocessable Content  : Echec de la validation des donnees`

## Mot de passe oublie

Ce endpoint permet de recevoir le lien de reset password par email.
Les champs attendus sont les suivants:

- email

Ce endpoint retourne un message email au destinataire.

La suite se passe au point [Nouveau mot de passe (suite Mot de passe oublie)](#nouveau-mot-de-passe-suite-mot-de-passe-oublie)


### HTTP Request

`POST http://example.com/forgot-password`


### Parametres POST 

| Parametre          | Description                      |
|--------------------|----------------------------------|
| email (string 255) | l'adresse email de l'utilisateur |


### Code HTTP retourne

`200 Ok : Pas de probleme`

`422 Unprocessable Content  : Echec de la validation des donnees (format ou non existance user)`

## Nouveau mot de passe (suite Mot de passe oublie)

Ce endpoint permet de recevoir le lien de reset password par email.
Les champs attendus sont les suivants:

- email
- token
- password
- password_confirmation

Ce endpoint retourne un message email au destinataire.



### HTTP Request

`POST http://example.com/reset-password`


### Parametres POST 

| Parametre          | Description                             |
|--------------------|-----------------------------------------|
| email (string 255) | l'adresse email de l'utilisateur        |
| token (string 255) | le token recu par email                 |
| password (string 255) | le nouveau mot de passe                 |
| password_confirmation (string 255) | la confirmation du nouveau mot de passe |


### Code HTTP retourne

`200 Ok : Pas de probleme`

`422 Unprocessable Content  : Echec de la validation des donnees (voir message particuler)`

```json
{
    "message": "Ce jeton de réinitialisation du mot de passe n'est pas valide.",
    "errors": {
        "email": [
            "Ce jeton de réinitialisation du mot de passe n'est pas valide."
        ]
    }
}
```


## Nouvelle adresse mail

Ce endpoint permet de modifier l'adresse mail de l'utilisateur connecte.
Ce endpoint permet de recevoir le lien de validation de la nouvelle adresse mail par email.
Les champs attendus sont les suivants:

- email

Ce endpoint retourne un message email au destinataire.

La suite se passe au point [Nouveau mot de passe (suite Mot de passe oublie)](#nouveau-mot-de-passe-suite-mot-de-passe-oublie)


### HTTP Request

`PATCH http://example.com/api/user/settings/email`


### Parametres PATCH 

| Parametre          | Description                      |
|--------------------|----------------------------------|
| email (string 255) | l'adresse email de l'utilisateur |


### Code HTTP retourne

`200 Ok : Pas de probleme`

`422 Unprocessable Content  : Echec de la validation des donnees (format ou non existance user)`

## Nouveau mot de passe (suite Mot de passe oublie)

Ce endpoint permet de recevoir le lien de reset password par email.
Les champs attendus sont les suivants:

- email
- token
- password
- password_confirmation

Ce endpoint retourne un message email au destinataire.



### HTTP Request

`POST http://example.com/reset-password`


### Parametres POST 

| Parametre          | Description                             |
|--------------------|-----------------------------------------|
| email (string 255) | l'adeesse email de l'utilisateur |
| token (string 255) | le token recu par email |
| password (string 255) | le nouveau mot de passe |
| password_confirmation (string 255) | la confirmation du nouveau mot de passe |


### Code HTTP retourne

`200 Ok : Pas de probleme`

`422 Unprocessable Content  : Echec de la validation des donnees (voir message particuler)`

```json
{
    "message": "Ce jeton de réinitialisation du mot de passe n'est pas valide.",
    "errors": {
        "email": [
            "Ce jeton de réinitialisation du mot de passe n'est pas valide."
        ]
    }
}
```













