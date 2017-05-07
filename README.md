Docker based Continuous Integration Containers
=======================================
Pre-Requisites
-------------
- Docker (for Mac OSX / Windows / Docker)
- Docker Compose
- Clone the repository

QuickStart
-----------
- To Start containers in the background

	`` 
	$ docker-compose up -d
	``
- Stoping all containers

	`` 
	$ docker-compose down
	``
	
The result of the compose is multi-fold
---------------------------------------
- Build Jenkins with Java8,Maven 3 , Nexus , Postgres , SonarQube (5.6.x) and Nginx Images
- Start the respective containers with port mapping
- Configure SonarQube with Postgres
- Mount docker volumes for persisting the changes done at container level
- Configure nginx reverse-proxy with urls relative to localhost

- Jenkins: http://localhost:8080/jenkins/
- Nexus: http://localhost:8080/nexus/
- Sonar: http://localhost:8080/sonar/

"Unlock Jenkins" - Initial Access
----------------------------------------------------------
Shell access into Jenkins Container

``
$docker exec -it jenkins /bin/bash
``

``
$cat /var/jenkins_home/secrets/initialAdminPassword
``

Copy and Paste the Value into Unlock Screen in Jenkins Web Console

To Start Individual Application Containers
-------

Jenkins
-------------------
docker run --name jenkins -p 10001:8080 -p 50000:50000 -d jenkins-jdk8 --prefix=/jenkins/

Nexus
-----------------
docker run -d --name nexus-db sonatype/nexus /bin/true
docker run -e "CONTEXT_PATH=/nexus" -d -p 10002:8081 --name nexus --volumes-from nexus-db sonatype/nexus

Sonarqube (with Postgres DB):
------------------------------------
docker run -d --name postgres-volume postgres /bin/true
docker run -d -e "SONARQUBE_WEB_CONTEXT=/sonar" --name sonarqube -p 10003:9000 -p 9092:9092 sonarqube "-Dsonar.web.context=/sonar"
docker run -d --name postgres --volumes-from postgres-volume -e "POSTGRES_USER=sonar" -e "POSTGRES_PASSWORD=sonar" --net container:sonarqube postgres

Nginx based Proxy:
-----------------------------
docker run --name proxy --link jenkins --link nexus --link sonarqube -d -p 8080:80 -d proxy

