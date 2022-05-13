# Keycloak 18.0.0 avec .NET 6
## Installer le serveur
1. Télécharger keycloak-11.0.3.[zip|tar.gz] à partir de Keycloak downloads.
2. Placer le fichier dans un répertoire que vous choisissez.
3. Décompresser le fichier ZIP à l'aide de l'utilitaire de décompression approprié, tel que jar, tar ou unzip.

## Démarrer le serveur
Aller dans le dossier bin et exécuter la commande suivante :
```kc.bat start-dev```

## Créer un administrateur
1. Accéder à l’adresse suivante : http://localhost:8080/.
2. Créer un administrateur dans Administration Console.

## Accéder à la console d'administrateur
1. Accéder à l’adresse suivante : http://localhost:8080/.
2. Cliquer sur Administration Console.
3. Entrer le mot de passe d’administrateur.

## Configurer l’audience dans Keycloack
- Ajouter realm "keycloak-demo".
- Ajouter un client “my-app”.
- Accéder au menu "Client Scopes".
  - Ajouter un nouveau Client scope “good-service”.
  - Accéder au Mappers dans les paramètres de “good-service”.
  - Créer un Mapper avec le nom “my-app-audience”.
      - Name: my-app-audience
      - Mapper type: Audience
      - Included Client Audience: my-app
      - Add to access token: on
  - Configurer le client “my-app” dans le menu “Clients”.
    - Accéder au Client Scopes dans les paramètres de “my-app”.
    - Ajouter Available Client Scopes "good-service" à  Assigned Default Client Scopes.

## Créer un utilisateur 
- Accéder au menu Users.
- Cliquer sur Add user.
  - Remplir le champ Username  par “user”.
  - Accéder au Credentials dans les paramètres de “user”.
  - Entrer et confirmer le mot de passe “test1234”.
  - Choisir OFF pour Temporary.

## Utilisation
- Exécuter la solution.
- Via postman ou tout autre client REST, vous devriez pouvoir obtenir le jeton et appeler l'API sécurisée :
```
curl \
  -d "client_id=my-app" \
  -d "username=user" \
  -d "password=test1234" \
  -d "grant_type=password" \
  "http://localhost:8080/realms/keycloak-demo/protocol/openid-connect/token"
  ```
  - Vous devriez alors pouvoir appeler l'API sécurisée : 
```
curl --location --request GET 'https://localhost:7017/WeatherForecast' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJaWmctYXFxOWFueS1VaTBYUDIydV9XNGlRcWhxWDBEWHBFLTQyM0lIRFNNIn0.eyJleHAiOjE2NTIzNjM1NTQsImlhdCI6MTY1MjM2MzI1NCwianRpIjoiODIxNGNkYzEtMTIzZi00NDgwLWFlOTYtMTk3NGEyZTE5YzAyIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL3JlYWxtcy9rZXljbG9hay1kZW1vIiwiYXVkIjoibXktYXBwIiwic3ViIjoiZDk0MDZhMjctNTM2My00N2JiLTgyMTYtNDNjMTg4ZTJiNDZlIiwidHlwIjoiQmVhcmVyIiwiYXpwIjoibXktYXBwIiwic2Vzc2lvbl9zdGF0ZSI6Ijg3ODNjNjUyLTViZTYtNGE0YS1hYWU0LTgyNDkyODcxZTI5MyIsImFjciI6IjEiLCJhbGxvd2VkLW9yaWdpbnMiOlsiKiIsImh0dHA6Ly9sb2NhbGhvc3Q6NDIwMCJdLCJzY29wZSI6InByb2ZpbGUgZ29vZC1zZXJ2aWNlIGVtYWlsIiwic2lkIjoiODc4M2M2NTItNWJlNi00YTRhLWFhZTQtODI0OTI4NzFlMjkzIiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJ1c2VyIn0.D3a4EXctKyYBadcBrW17ph4d-CFlKUtEtgaq8O4IfpXBS6sgqb8hsk_BPyuoxkeHhg-Xi5T9sFOPx0QmPRDxWdFc4g5xfHRW7ox2AtSuFqoG1mThp6AvO5Pi-CO_hUg4kvWUlGdaEds2nUmd92xH32QHkwKIGzw4d4iFq_rIT_IpM5P5U5bV3aPYOzve7R2d2YkUZ6kOVoUUl8fAMvwL4J4nOHrkkf243D-yMetprcibtWV4BNS6osL0z3cT_RvIDkV6KnN5pHJ2esutxE_b-HdluBgF3whDY7du05aZ8JxAlvICU0hnPbBryc-P5JEhLc3uOqJHxLKfE4grV2naOg'
```
