# Docker, MySQL et GRAFANA : Tableau de bord sur la Covid (Brief n°15)
Utiliser un conteneur Grafana, grâce à Docker, pour faire de la Dataviz.

**************************************************************************************************************
## Contexte du projet
Afin de préparer une stack facilement déployable pour l'équipe de dev, les analystes data et le client, votre chef de projet vous demande de préparer des conteneurs pour des outils de Dataviz. Après discution avec la haute autorité de santé, votre client, les technologies retenues sont MySQL et GRAFANA. Le serveur est en Linux.


**************************************************************************************************************
## Documents dans ce GitHub :
- une capture d'écran du dashborad GRAFANA,
- le docker compose de la stack,
- ce README détaillé du travail


**************************************************************************************************************
## Explications du docker-compose.yaml :

1) **Création des services :**
      - mysqldb
      - adminer
      - grafana
      - volumes
      - networks


2) **dans le service mysql:**
      -  Pour lier avec le service volume, et je mets son nom : `- /mysqldb:/var/lib/mysql`
      - Pour importer directement la database : `- ./data/vaccination.sql:/docker-entrypoint-initdb.d/vaccination.sql`

3) **Création d'un service "adminer" pour gérer la database** 

4) **dans le service grafana**
      - crétion d'un volume pour stocker et sauvegarder les graphiques : `- grafana-storage:/var/lib/grafana`          

5) **service volume qui reprend les volumes de mysql et de grafana**
  

**************************************************************************************************************
## Graphiques :
Nous avons tracé 4 graphes pour donner une vue d'ensemble de la situaition Covid à partir des données disponibles portant sur la période du 20 décembre 2020 au 20 février 2021.  Voici la Requete SQL utilisée avec des bar chars:

Voici le tableau de bord de la situation
      ![graphique1](https://github.com/MainaLD/Docker_tableau_bord_Covid/blob/main/grafana-storage/Dashboard-Analyse_Covid19_-_Grafana0406.png)

Voici le détails:
- **Vue d'ensemble de la vaccination dans le monde**

![Nb_total_de_vaccns_dans_le_monde](https://github.com/MainaLD/Docker_tableau_bord_Covid/blob/main/grafana-storage/Nb_total_de_vaccins_produits_en_millions_-_Grafana0406.png)
     
     `SELECT daily_vaccinations_per_million AS "time", total_vaccinations
      FROM country_vaccinations
      ORDER BY daily_vaccinations_per_million;`
      
- **Situation de la France**

![Situation vaccinale en France](https://github.com/MainaLD/Docker_tableau_bord_Covid/blob/main/grafana-storage/Situation%20vaccinale%20en%20France-%20Grafana%200406.png)
      
      `SELECT total_vaccinations, people_vaccinated, people_fully_vaccinated
      FROM country_vaccinations
      WHERE country = "France";`
      
 - **Palmares de 10 pays qui ont le plus vaccines**

![Palmares_des_10_premiers_pays_les_plus_vaccines](https://github.com/MainaLD/Docker_tableau_bord_Covid/blob/main/grafana-storage/Palmares_des_10_premiers_pays_les_plus_vaccines_-_Grafana0406.png)
      
      `SELECT country, sum(total_vaccinations) as 'somme'
      FROM country_vaccinations
      GROUP By country
      ORDER BY somme desc
      LIMIT 10;`
      
 - **Les principaux consortiums de producteurs de vaccins en terme de production**
 
 ![Nb_total_de_vaccins_produits_en_millions](https://github.com/MainaLD/Docker_tableau_bord_Covid/blob/main/grafana-storage/Nb_total_de_vaccins_produits_en_millions_-_Grafana0406.png)
      
      `SELECT vaccines, sum(total_vaccinations) as 'somme'
      FROM country_vaccinations
      GROUP By vaccines
      ORDER BY somme desc;`








