# OnCall Management Application

# Owerview

Application permettant la visualisation et gestions des astreintes d'une entreprise.

# Features

* Plannification des astreintes
* Visualisation des plannings d'astreintes
* Notification par email des astreintes pour la semaine à venir
* Demande d'enregistrement en paye d'une astreinte
* Rappel par email pour la finalisation d'une demande de paiement lorsqu'une astreinte est terminée.
* Validation électronique d'une demande d'enregistrement en paye.
* Impression d'une demande d'enregistrement en paye (uniquement par les RHs)

# MVP

## Roles

| Role              | Role ID | Définition                                                   |
| ----------------- | ------- | ------------------------------------------------------------ |
| User              | user    | Employée effectuant des astreintes                           |
| Manager           | manager | Responsable hiérarchique, responsable d'équipe...            |
| Ressource Humaine | hr      | Responsable de la paie                                       |
| Administrateur    | admin   | Administrateur de l'application, responsable des configurations du logiciel. |


## API models

CRUD sur tous les data model.

## Data Models

### User

```js
{
  "id": 1,
  "firstname": "Judes",
  "lastname": "HONORE",
  "email": "judes.honore@outlook.com",
  "registration_number": 49494949, // Numéros de matricule
  "role_id": 4
}
```

#### Role

> Voir chapitre sur les [roles](##Roles)

```js
{
  "id": 1,
  "name": "user",
  "description": "Employée effectuant des astreintes"
}
```



### Service

```js
{
  "id": 1,
  "name": "marketing",
  "manager_id": 1  // User that manage this service
}
```



### Oncall

```js
{
  "id": 1,
  "user_id": 1,
  "quotation_id": 1,
  "start_date": "20200101T00:00:00-000Z",
  "end_date": "20200101T00:00:00-000Z",
  "week_number": 10,   // The week number,
  "createdAt": "20200101T00:00:00-000Z",
  "updatedAt": "20200101T00:00:00-000Z"
}
```



### Quotation

> Demande d'enregistrement/paiement d'une astreinte
>
> Cette demande est créer automatiquement lorsque q'une astreinte est plannifier.
>
> Lorsque que la date d'une astreinte est dépasser un mail de rappel.

```js
{
  "id": 1,
  "user_id": 1,
  "oncall_id": 1,
  "approved_by": 1, // Id du manager qui a valider la demande
  "approved": false,  // Demande de paiment approuver ou non par le manager,
  "printed": false,   // Status pour aider les RHs dans la gestions des astreintes
  "oncall_type": 1,   // Type de l'astreinte enum("OP", "AS", "TE")
  "increased_hours": 3, // Nombre d'heures majorée du mois
  "previous_increased_hours": 3, // Nombre d'heures majorée du mois
  "balance_to_carry": 0, // Solde des heure a reporter sur le mois prochain
  "compasation_type": 1 // Type de la compensation enum(OPt1, OPt2) 
  "validateddAt": "20200101T00:00:00-000Z",
  "createdAt": "20200101T00:00:00-000Z",
  "updatedAt": "20200101T00:00:00-000Z"
}
```

#### Compensation Type

> Données : 
>
> Option1 = compensation monétaire
>
> Option2 = compasation horraire

```js
{
  "id": 1,
  "name": "option1",
  "description": "Compensation sous forme de paiement forfait demi-journée"
}
```

#### Oncall Type

> Données :
>
> AS = astreinte
>
> OP = opération
>
> TE = travail exeptionel

```js
{
  "id": 1,
  "name": "AS",
  "description": "Astreinte"
}
```

#### Intervention

```js
{
  "id": 1,
  "oncall_id": 1,
  "subject": "problème sur l'api gateway",
  "ticket_id": 192345,
  "week_day": 0, // Numéros du jours de la semaine concernée enum(0(lundi)->6(dimanche))
  "is_public_holiday": false,  // Est-ce une intervention un jour férié ?
  "is_day": true, // Intervention de jours ou de nuit ?
  "started_at": "20200101T00:00:00-000Z",
  "ended_at": "20200101T00:00:00-000Z",
  "duration": 1, // Temps d'intervention en heures
  "createdAt": "20200101T00:00:00-000Z",
  "updatedAt": "20200101T00:00:00-000Z"
}
```

##### Intervention Summary

> Récaptitulatif des interventions.
>
> Contient le nombre de d'heure majorées et le nombre d'heures travaillées

```js
{
  "id": 1,
  "intervention_id": 1,
  "worked_hours": 12,    // Nombres d'heures travaillées de la semaine
  "increased_hours": 24 // Nombres d'heures majorées de la semaine
}
```



##### Intervention coefficient

> Liste des coefficients:
>
> jour=100, isPublicHoliDay=false, isDay=true
>
>  nuit=150,  isPublicHoliDay=false, isDay=false
>
> Dimanche=200, isPublicHoliDay=false, isDay=true, isSunday=true
>
> jour férié=200, isPublicHoliDay=true, isDay=true, isSunday=false
>
> nuit dimanche=210, isPublicHoliDay=false, isDay=false, isSunday=true
>
> nuit jour férier=210 , isPublicHoliDay=true, isDay=false, isSunday=false

```js
{
  "id": 1,
  "name": "jour",
  "value": 100,
  "is_public_holiday": false,
  "is_day": true,
  "is_sunday": false,
}
```



### MailingList

>  Liste des adresses email pour les notifications

```js
{
  "id": 1,
  "email": "jhon.doe@mail.com",
  "description": "prestataire "
}
```




