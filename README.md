# Accident-prediction
Application d'évaluation de la gravité des accidents de la route à l'aide du Machine Learning

Les accidents de la route représentent une problématique majeure en matière de sécurité publique, mobilisant quotidiennement les forces de l’ordre pour intervenir rapidement et efficacement. La gestion des interventions sur les lieux d’accidents est un défi complexe, nécessitant une évaluation rapide de la gravité de la situation afin de déployer les ressources adéquates. Dans ce contexte, les policiers doivent souvent prendre des décisions cruciales avec des informations limitées, ce qui peut compromettre l'efficacité de leur réponse.

## 1)	Contexte et Objectifs
   
Notre application est destinée à répondre aux besoins des forces de l’ordre en leur offrant un outil capable d’évaluer rapidement et de manière fiable la gravité potentielle d’un accident de la route. Conçue pour optimiser les interventions des policiers, elle facilite la prise de décision sur le terrain et la préparation des moyens nécessaires avant leur arrivée sur les lieux. Elle vise à réduire les erreurs humaines liées à une évaluation rapide, ce qui permet aux équipes d'intervenir de manière plus ciblée. Commanditée par une entité publique, comme le ministère de l’Intérieur ou une autorité nationale de la police, elle sera spécifiquement utilisée par les policiers intervenant sur les accidents.

Pour répondre aux contraintes opérationnelles, l’application sera déployée sous la forme d’un site web sécurisé, accessible via des appareils mobiles tels que des smartphones et des tablettes. Ce format garantit une utilisation simple, économique et flexible, tout en permettant des mises à jour centralisées. Afin d’assurer une protection optimale des données sensibles et de réduire les coûts liés aux solutions cloud, l’hébergement sera réalisé sur un serveur interne. Enfin, grâce à son interface graphique intuitive, l’application communiquera avec une API côté serveur hébergeant le modèle de prédiction, assurant un échange fluide et efficace des données nécessaires à l’estimation de la gravité des accidents.


## 2)	Modèle

Le modèle employé pour notre application est un modèle d'apprentissage automatique Random Forest, qui a montré les meilleures performances pour notre problème de classification. Ce modèle prend en entrée 28 caractéristiques liées à un accident de la route et prédit la gravité de l'accident. La variable cible est binaire, où 1 représente un accident grave (prioritaire) et 0 un accident moins grave (non prioritaire). L’objectif principal est de détecter avec précision les accidents graves afin d'assurer une intervention rapide et prioritaire. Random Forest, avec sa capacité à gérer des données complexes et à éviter le sur-apprentissage, est particulièrement adapté à ce type de classification.

Concernant les métriques d’évaluation du modèle, nous avons choisi d’utiliser le F1-score, qui combine la précision et le rappel, pour évaluer l’équilibre entre ces deux critères. Après un grid search pour ajuster les paramètres, le modèle a obtenu un F1-score de 0,6. Ce résultat est satisfaisant pour l'objectif de classification binaire, mais il peut être amélioré. Afin de mieux identifier les incidents les plus critiques, il est recommandé de renforcer l’accent sur la précision du modèle concernant les accidents graves. Le modèle présente un temps d’entraînement de 0,79 seconde et un temps de prédiction de 0,19 seconde, ce qui est suffisamment rapide pour une utilisation en temps réel sur le terrain. Toutefois, étant donné l'importance de bien détecter les accidents graves, il serait judicieux d'ajouter le rappel comme métrique supplémentaire, afin de s'assurer que les incidents nécessitant une intervention urgente soient correctement identifiés.


## 3)	Base de données

Le jeu de données initial comprend 89 tables accompagnées d’une documentation détaillée. Ces données proviennent du Fichier BAAC, géré par l’Observatoire national interministériel de la sécurité routière (ONISR) et sont accessibles via la plateforme data.gouv.fr. Le Fichier BAAC rassemble les informations collectées par les unités des forces de l’ordre (police, gendarmerie, etc.) lors des interventions sur les lieux des accidents corporels. Chaque accident corporel, défini comme un accident survenu sur une voie publique, impliquant au moins un véhicule et causant une victime nécessitant des soins, fait l’objet d’une saisie d’informations dans une fiche intitulée bulletin d’analyse des accidents corporels.

Les données couvrent la période de 2005 à 2022 et sont organisées en quatre grandes tables :

•	CARACTÉRISTIQUES : Informations générales sur les circonstances de l’accident.

•	LIEUX : Caractéristiques du lieu de l'accident.

•	VÉHICULES : Caractéristiques des véhicules impliqués.

•	USAGERS : Caractéristiques des personnes concernées par l'accident.

Pour notre analyse, nous avons restreint notre étude aux données de l’année 2021. Le Fichier BAAC est mis à jour chaque année et reste accessible au grand public via le site officiel, garantissant ainsi un accès continu et actualisé aux données sur les accidents corporels de la circulation.
Les mises à jour fréquentes du fichier garantissent également que le modèle bénéficie de données récentes pour ajuster ses prédictions aux tendances actuelles.
Gestion de données :

Toutes ces données seront stockées sous leur forme brute sur un serveur et manipulées à l'aide de SQLite, choisi pour sa légèreté et son efficacité dans la gestion des bases de données. Par la suite, un traitement sera effectué afin de rendre ces données exploitables par notre modèle, suivant une approche de type ELT (Extract, Load, Transform).

Le délai d'une année pour les nouvelles acquisitions de données est trop long et pourrait limiter l'efficacité du modèle. Il serait préférable de fractionner les fichiers BAAC obtenus annuellement en fichiers trimestriels. Ces fichiers serviront à entraîner le modèle de manière plus fréquente et périodique.

Afin d’évaluer les performances de notre modèle en temps réel, nous prévoyons de mettre en place une base de données dédiée au feedback, qui centralisera les retours des utilisateurs, les caractéristiques des accidents, ainsi que les prédictions générées par le modèle. Cela permettra d’ajuster rapidement les performances du modèle en fonction de l’évolution des conditions réelles.


## 4)	API

Notre API offrira une gamme de fonctionnalités pour répondre aux divers besoins des utilisateurs. Toutefois, certaines de ces fonctionnalités seront exclusivement accessibles aux administrateurs afin de garantir un contrôle renforcé et une sécurité optimale.
Les fonctionnalités réservées aux administrateurs incluront notamment :

•	La sécurisation de l’API

•	La gestion des utilisateurs (ajout, modification, suppression)

•	La mise à jour de la base de données

•	L’entraînement du modèle

•	Le suivi et le monitoring des performances du modèle

Pour les utilisateurs non-administrateurs, l’API proposera des outils adaptés à leurs besoins, tels que :
•	L’accès aux prédictions du modèle

•	La vérification du bon fonctionnement du système

Pour garantir une gestion efficace de l’authentification et des droits d’accès, SQLite sera utilisé comme base de données pour stocker les informations liées à l’authentification des utilisateurs.

## 5)	Testing :

•	Tester le bon fonctionnement du modèle lors de l’entraînement :

-	Vérifiez que les données d'entraînement sont correctement préparées et ne contiennent pas d'erreurs susceptibles de nuire à l'apprentissage du modèle.
	  
-	Confirmez que les hyperparamètres choisis (par exemple, max_depth, min_samples_split, etc.) sont bien appliqués pendant le processus d'entraînement.
   
-	 Évaluez le modèle pour vous assurer qu'il fonctionne comme prévu.

•	Tester le bon fonctionnement du modèle lors de la prédiction 

-	S'assurer que le modèle prédit avec précision la gravité des accidents en utilisant les données de test.
  
-	Évaluer la performance des prédictions, en accordant une attention particulière à la classe prioritaire (accidents graves), afin de minimiser les erreurs de classification susceptibles d'affecter la fiabilité du modèle .
  
-	Vérifier que les prédictions sont retournées dans un délai acceptable 

•	Tester le bon fonctionnement des différents endpoints de l’API:

-	 Vérifier le bon fonctionnement de chaque endpoint de l'API (par exemple, prédiction de la gravité, mise à jour des utilisateurs, etc.) en s'assurant qu'ils répondent correctement et renvoient les résultats attendus.
  
-	Soumettre des entrées incorrectes aux endpoints pour vérifier la gestion efficace des erreurs par l'API (par exemple, données mal structurées ou manquantes).
  
-	Contrôler les mécanismes de sécurité de l'API, notamment en testant l'authentification et l'autorisation des utilisateurs.
  
•	Tester le bon fonctionnement du processus d’ingestion de nouvelles données :

-	Vérifier que les données ingérées respectent le format attendu (schéma, types de colonnes, etc.).
  
-	Tester les prédictions avec des valeurs extrêmes ou des entrées partielles.
  
-	Assurer que le modèle ne plante pas en cas d’entrée inattendue (valeurs nulles, chaînes de caractères au lieu de nombres, etc.)

  
## 6)	Monitoring
   
Pour garantir la performance et l’adaptabilité de notre modèle, nous avons mis en place une stratégie hybride, combinant des réentraînements réguliers et une surveillance continue des performances.


## 7) Réentraînement du modèle

Le modèle est réentraîné périodiquement, par exemple, tous les trimestres. Cette approche préventive permet de maintenir la pertinence du modèle en intégrant les nouvelles données, même en l’absence de signes explicites de dégradation. Parallèlement, un suivi en production est assuré à l’aide de métriques clés telles que le rappel et le F1-score. Si une baisse critique de performance est détectée, un réentraînement anticipé est déclenché.

L’utilisation de réentraînements trimestriels permet également d'intégrer des changements saisonniers ou tendances spécifiques qui pourraient influencer les accidents de la route.

Pour ces réentraînements, nous adoptons une stratégie de données combinée. Lors des réentraînements réguliers, nous utiliserons un jeu mixte comprenant l’intégralité des données historiques, ainsi qu’un échantillon des données récentes transmises annuellement sous forme de fichiers trimestriels. Cette approche permet d’assurer une bonne généralisation du modèle tout en intégrant régulièrement les évolutions récentes des données. En revanche, pour les réentraînements anticipés, en cas de dégradation significative des performances, nous privilégierons les données récentes récupérées directement à partir de la base de données feedback, alimentée par les retours des utilisateurs. Cette méthode garantit une réactivité rapide pour corriger les dérives et adapter le modèle aux changements immédiats des données.


## 8) Évaluation de la performance
   
La performance du modèle est évaluée de manière hybride pour répondre aux besoins de robustesse et de réactivité.

•	Évaluation globale : réalisée sur l’ensemble du jeu de test, elle mesure la capacité du modèle à généraliser sur le long terme et dans divers scénarios. Cette évaluation est particulièrement utile pour des audits réguliers et les comparaisons interversions du modèle.

•	Évaluation ciblée : axée sur les données les plus récentes, elle détecte rapidement les dérives potentielles ou les changements dans les relations entre les variables. Elle permet de cibler les adaptations nécessaires dans un contexte dynamique, par exemple, après une modification majeure des comportements ou des tendances.

Cette double approche garantit une évaluation complète, permettant à la fois de maintenir des performances solides à long terme et de réagir rapidement aux évolutions des données.


## 9) Gestion des baisses de performance
    
En cas de perte de performance significative du modèle, plusieurs actions sont prévues en fonction de la gravité :

•	Alerte : un mail est envoyé aux responsables concernés pour les informer de la situation et coordonner une réponse rapide, y compris la réévaluation des paramètres du modèle et, si nécessaire, la mise en place d'un réentraînement.

•	Blocage temporaire : si les prédictions deviennent trop peu fiables, des mesures de sécurité, comme le blocage temporaire de l’application, peuvent être envisagées.

•	Amélioration continue : après chaque détection de dérive, une analyse approfondie des causes sera effectuée. Des ajustements spécifiques du modèle seront mis en place, suivis par un réentraînement avec de nouvelles données ou la mise à jour des variables utilisées. Les itérations successives permettront d'améliorer le modèle de manière continue, renforçant ainsi sa fiabilité à long terme.

Cette approche pragmatique permet de limiter les impacts potentiels tout en rétablissant rapidement la fiabilité du modèle.

En combinant réentraînements réguliers, surveillance proactive, évaluations complètes et un plan d’action en cas de dérive, nous assurons la robustesse et l’adaptabilité continue de notre modèle face aux évolutions des données et aux exigences opérationnelles.


# Conclusion

En résumé, la gestion de la performance et des baisses de performance du modèle repose sur une approche rigoureuse et adaptable. L’évaluation continue, à la fois globale et ciblée, permet de suivre de près l’évolution du modèle dans des scénarios variés, assurant ainsi sa robustesse à long terme. En cas de dérive ou de perte de performance, un système d'alertes et de mesures correctives, comme le réentraînement et l'ajustement des paramètres, garantit une réactivité optimale. Cette méthode hybride, combinant prévention et réactivité, assure que le modèle reste performant, fiable et aligné avec les exigences opérationnelles et les changements de données, contribuant ainsi à une prise de décision éclairée et durable.

# Machine Learning Canvas

Consultez le [Machine Learning Canvas](Machine_Learning_Canvas.md) pour plus de détails.


