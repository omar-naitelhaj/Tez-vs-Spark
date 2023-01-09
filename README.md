# Comparaison entre Apache Tez et Apache Spark en termes de traitement de données et l’automatisation d’un pipeline de données en utilisant Apache Tez.


## 1-	Installation

Voici le contenu du fichier « docker-compose.yml », il est composé de 12 services, « zeppelin », « namenode », « datanode », « spark-master », « spark-worker », « hue», « pig », « hive-server », « hive-metastore », « hive-metastore-postgresql », « zookeeper », « presto-coordinator »

![image](https://user-images.githubusercontent.com/77858969/211403743-b8ccca6f-308d-4802-94c3-fc48cd42ba79.png)

Afin de lancer toutes les images créées dans des conteneurs dans le même cluster, on tape la commande suivante : « docker-compose up -d »

![image](https://user-images.githubusercontent.com/77858969/211403877-95a1b6c2-ed78-4a6a-8cf3-30a266470442.png)

Apres on doit verifier que tous les service running:

![image](https://user-images.githubusercontent.com/77858969/211403969-2b8a30ef-229d-45b5-b64c-1a3943ea4449.png)

## 2-	Configuration

On accède au bash du service « zeppelin »

![image](https://user-images.githubusercontent.com/77858969/211404135-4366a9ba-d174-49fc-8d98-538a7ca57ab3.png)

On se déplace au sein du dossier « jdbc », et on installe les deux « Jars », « hadoop-common-2.6.0.jar » et « hive-jdbc-2.3.2-standalone.jar »

![image](https://user-images.githubusercontent.com/77858969/211404325-bb6b6f9f-29bc-4622-81a8-0f2c7cf7ec18.png)
![image](https://user-images.githubusercontent.com/77858969/211404345-0249588a-47b0-40e7-b3c1-4025f9a188dc.png)

Dans zeppelin, on crée un nouveau interpreteur “hive”, on choisit le groupe “jdbc”, et on ajoute la configuration suivante

![image](https://user-images.githubusercontent.com/77858969/211404404-ed8eec25-cf90-4ccc-81f0-c1291d74c06b.png)
![image](https://user-images.githubusercontent.com/77858969/211404434-831e8b65-42f6-439b-8f9e-f04dcbb003a2.png)

Il nous reste que copier le dataset dans l’OS du container « zeppelin », afin d’y accéder via le notebook « spark »

![image](https://user-images.githubusercontent.com/77858969/211404509-5a28f8fc-403c-4e16-9fa1-8128ed41e18c.png)

## 3-	Manipulation avec Spark/Hive/Tez

On va tester les commandes suivantes dans SparkSQL et Hive avec mapreduce et avec tez et aussi Pig avec MR et avec tez:
<ul>
  <li>SELECT * FROM diabetes</li>
  <li>SELECT age,count(*) FROM diabetes GROUP BY age</li>
  <li>SELECT age,avg(BMI),avg(Insulin),avg(SkinThickness) FROM diabetes GROUP BY age</li>
  <li>SELECT age,avg(Outcome) as meanOutcome FROM diabetes GROUP BY age ORDER BY meanOutcome4</li>
</ul>

Pour hive avec map reduce on va utuliser : set hive.execution.engine = mr;
Pour hive avec tez on va utuliser : set hive.execution.engine = tez;

Pour pig avec map reduce c'est le moteur par defaut.
Pour pig avec tez on va changer l'interpreter:

![image](https://user-images.githubusercontent.com/77858969/211406485-93c0e631-ba5a-4f99-ba3b-69a61e6374f1.png)

## 4-	Pipeline ETL

Pour la partie d’apache airflow, on lance la première commande de docker-compose pour airflow init :

![image](https://user-images.githubusercontent.com/77858969/211406976-d2f3b5bb-3822-48e4-9163-5cb3014a2a4a.png)

Puis on lance le docker-compose up pour executer notre contenaire :

![image](https://user-images.githubusercontent.com/77858969/211407032-dbe1614a-7b1d-463e-a02a-32ab40f9ef46.png)

Notre centenaire est en cours d’exécution :

![image](https://user-images.githubusercontent.com/77858969/211407091-c493aaf0-4c1b-4456-9eb3-80db37279781.png)

Initialisation des arguments par default qu’on va passer au notre DAG ainsi que notre DAG :

![image](https://user-images.githubusercontent.com/77858969/211407236-afb75d1d-d868-4264-8e78-56b4a0d928fb.png)

Définition de la première Task et la deuxième Task :

![image](https://user-images.githubusercontent.com/77858969/211407266-3db4269a-e1a2-45cc-875c-369f003301c2.png)

La tâche de l’extraction et Transformation et l’upload du DATA dans un fichier CSV:

![image](https://user-images.githubusercontent.com/77858969/211407320-f3d23818-ebd5-434f-991e-53af242e3bd2.png)

La tâche de l’upload du data dans une base de données :

![image](https://user-images.githubusercontent.com/77858969/211407412-a9abe42e-32d0-4182-a6de-964afdb054a0.png)

Et donc la résultat en sqlLite doit affiche comme ca :

![image](https://user-images.githubusercontent.com/77858969/211407486-942dc96f-6716-4691-b1bf-c8c65b19c3c5.png)
