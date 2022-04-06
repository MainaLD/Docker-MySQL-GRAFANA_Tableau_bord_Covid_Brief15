# Docker_tableau_bord_Covid
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
Explication :






**************************************************************************************************************
## Graphiques :
Nous avons tracé 4 graphes pour donner une vue d'ensemble de la situaition Covid à partir des données disponibles portant sur la période du 20 décembre 2020 au 20 février 2021.  Voici la Requete SQL utilisée avec des bar chars:

Voici le tableau de bord de la situation
      ![graphique1](https://github.com/MainaLD/Docker_tableau_bord_Covid/blob/main/grafana01.JPG)

Voici le détails:
- Vue d'ensemble de la vaccination dans le monde
     grafana-storage/Nb_total_de_vaccns_dans_le_monde-_Grafana0406.png
     SELECT daily_vaccinations_per_million AS "time", total_vaccinations
      FROM country_vaccinations
      ORDER BY daily_vaccinations_per_million
- Situation de la France

      SELECT total_vaccinations, people_vaccinated, people_fully_vaccinated
      FROM country_vaccinations
      WHERE country = "France"
 - Palmares de 10 pays qui ont le plus vaccines
grafana-storage/Palmares_des_10_premiers_pays_les_plus_vaccines_-_Grafana0406.png
      SELECT country, sum(total_vaccinations) as 'somme'
      FROM country_vaccinations
      GROUP By country
      ORDER BY somme desc
      LIMIT 10; 
 - Les principaux consortiums de producteurs de vaccins en terme de production
      grafana-storage/Nb_total_de_vaccins_produits_en_millions_-_Grafana0406.png
      SELECT vaccines, sum(total_vaccinations) as 'somme'
      FROM country_vaccinations
      GROUP By vaccines
      ORDER BY somme desc;








