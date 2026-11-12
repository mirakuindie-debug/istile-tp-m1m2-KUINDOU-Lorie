# Fiche : requêtes HTTP et codes d'état

## Requête `https://dummyjson.com/users/1`

- **Méthode** : `GET`
- **Code d'état** : `200` (OK)
- **Type de contenu de la réponse** : `application/json; charset=utf-8`
- **Nom de deux champs du JSON reçu** : `firstName` (« Emily ») et `email` (« emily.johnson@x.dummyjson.com »)

## Requête `https://dummyjson.com/users/99999clear`

- **Code obtenu** : `400`
- **Il signifie** : *Bad Request*. Le serveur refuse la requête parce qu'elle est mal formée : l'identifiant attendu est un nombre, et `99999clear` n'en est pas un. Le serveur ne cherche même pas l'utilisateur.
- **Réaction de l'interface** : afficher un message clair (« Identifiant invalide » ou « Requête incorrecte ») et inviter à corriger la saisie. Il vaut mieux aussi valider l'identifiant côté client avant d'envoyer la requête, par exemple en vérifiant que c'est bien un nombre.

### Pour ne pas confondre 400 et 404

| Requête | Code | Cause |
|---|---|---|
| `/users/99999clear` | **400** | L'identifiant n'a pas un format valide. La requête est mal formée. |
| `/users/99999` | **404** | L'identifiant est valide, mais aucun utilisateur ne porte ce numéro. |

## Classer en côté client ou côté serveur

| Situation | Côté | Pourquoi |
|---|---|---|
| A. Afficher un message d'erreur sous un champ | **Client** | C'est de l'affichage, géré par le HTML, le CSS et le JavaScript du navigateur. |
| B. Enregistrer une note dans la base de données | **Serveur** | Seul le serveur accède à la base de données. |
| C. Vérifier un mot de passe | **Serveur** | La vérification se fait là où le mot de passe haché est stocké. Le client ne peut contrôler que le format. |
| D. Changer la couleur d'un bouton au survol | **Client** | C'est du CSS (`:hover`), sans échange avec le serveur. |
| E. Décider qu'un utilisateur n'a pas le droit de supprimer une note | **Serveur** | C'est une règle d'autorisation. Si elle n'existait que dans l'interface, elle serait facile à contourner. |

## Associer chaque code à une situation et à la réaction de l'interface

| Code | Nom | Situation | Réaction de l'interface |
|---|---|---|---|
| **201** | Created | Une ressource vient d'être créée, par exemple l'ajout d'une note avec `POST`. | Confirmer le succès (« Note enregistrée »), ajouter l'élément à la liste et vider le formulaire. |
| **401** | Unauthorized | L'utilisateur n'est pas authentifié : pas connecté, jeton absent ou expiré. | Rediriger vers la page de connexion avec un message du type « Veuillez vous connecter ». |
| **404** | Not Found | La ressource demandée n'existe pas, comme . | Afficher « Introuvable » ou une page 404, avec un lien de retour. |
| **500** | Internal Server Error | Le serveur a rencontré un bug ou une panne imprévue. | Afficher un message général (« Une erreur est survenue, réessayez plus tard ») sans détails techniques, et proposer de réessayer. |
