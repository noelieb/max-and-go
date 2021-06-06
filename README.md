# max-and-go

Cette application est destinée aux abonnés de TGVMax à la SNCF et permet de trouver des nouvelles destinations pour partir en week-end gratuitement grâce à leur abonnement.
Dans un Fragment de paramètres, on choisit une ville de départ, une date et une heure à laquelle on peut partir, et une date et une heure à laquelle on doit être rentré.
Par exemple, je sais que je veux partir en weekend depuis Paris, en partant le vendredi soir après 18h et en étant rentrée le dimanche soir à 23h dernier délai. 

Les champs renseignés par l'utilisateur vérifient la validité des données sur le champ de ville de départ, en implémentant un AutoCompleteTextView (rempli par les villes possibles de la SNCF).

La liste de weekends possible est crée dans un objet indépendant (WeekendList) via des appels à l'API de la SNCF, puis est implémentée dans une RecyclerView dans WeekendList.
En cliquant sur un weekend de la liste, l'utilisateur peut voir le détail du weekend, à savoir les heures de départ et d'arrivée des trains aller et retour permettant de rester le plus d'heures sur place.

Il y a eu une volonté de créer un Service qui tournerait en fond, permettant à l'application de chercher des nouvelles destinations tous les jours, en envoyant des notifications si ces dernières changent.
Ces implémentations, dans DailyFetch, ont effectivement mis en place l'envoi de notifications (grâce à un NotificationManagerCompat) et la nouvelle liste de week-ends  possibles se met à jour tous les jours grâce à une classe étendant BroadcastReceiver.

Ayant multiples appels à l'API en parallèle et donc en asynchrone, des Handler ont été utilisés pour déterminer le moment auquel tous les week-ends ont bien été trouvés.

Les stockages de données (comme par exemple des paramètres utilisateur : ville de départ,...) sont gérés en cache via des SharedPreferences.
