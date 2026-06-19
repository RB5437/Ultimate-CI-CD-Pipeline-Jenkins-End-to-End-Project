# Spring Boot based Java web application
 
This is a simple Sprint Boot based Java application that can be built using Maven. Sprint Boot dependencies are handled using the pom.xml 
at the root directory of the repository.

This is a MVC architecture based application where controller returns a page with title and message attributes to the view.

## Execute the application locally and access it using your browser

Checkout the repo and move to the directory

```
git clone https://github.com/RB5437/Ultimate-CI-CD-Pipeline-Jenkins-End-to-End-Project.git
cd java-maven-sonar-argocd-helm-k8s/sprint-boot-app
```

Execute the Maven targets to generate the artifacts

```
mvn clean package
```

The above maven target stroes the artifacts to the `target` directory. You can either execute the artifact on your local machine
(or) run it as a Docker container.

** Note: To avoid issues with local setup, Java versions and other dependencies, I would recommend the docker way. **


### Execute locally (Java 11 needed) and access the application on http://localhost:8080

```
java -jar target/spring-boot-web.jar
```

### The Docker way

Build the Docker Image

```
docker build -t ultimate-cicd-pipeline:v1 .
```

```
docker run -d -p 8010:8080 -t ultimate-cicd-pipeline:v1
```

Hurray !! Access the application on `http://<ip-address>:8010`


## Next Steps

### Configure a Sonar Server locally

```
System Requirements
Java 17+ (Oracle JDK, OpenJDK, or AdoptOpenJDK)
Hardware Recommendations:
   Minimum 2 GB RAM
   2 CPU cores
Complete Corrected SonarQube Installation (EC2 Ubuntu)
bash
# 1. System update + Java 17 install (SonarQube 26.x requires Java 17 or 21)
sudo apt update
sudo apt install -y unzip openjdk-17-jdk wget

# 2. Linux kernel settings — SonarQube runs Elasticsearch internally,
#    if these values aren't set, SonarQube will crash immediately on startup
echo "vm.max_map_count=262144" | sudo tee -a /etc/sysctl.conf
echo "fs.file-max=65536" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p

# 3. Set ulimits for the same reason (Elasticsearch needs higher file/process limits)
echo "sonarqube   -   nofile   65536" | sudo tee -a /etc/security/limits.conf
echo "sonarqube   -   nproc    4096" | sudo tee -a /etc/security/limits.conf

# 4. Create a dedicated non-root user (SonarQube refuses to start as root)
sudo adduser --system --no-create-home --group sonarqube

# 5. Download the latest version (26.4.0.121862, April 2026 release)
cd /opt
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-26.4.0.121862.zip
sudo unzip sonarqube-26.4.0.121862.zip
sudo mv sonarqube-26.4.0.121862 sonarqube
sudo rm sonarqube-26.4.0.121862.zip

# 6. Set ownership
sudo chown -R sonarqube:sonarqube /opt/sonarqube
sudo chmod -R 775 /opt/sonarqube

# 7. Start it as the non-root user (do NOT start as root)
sudo -u sonarqube /opt/sonarqube/bin/linux-x86-64/sonar.sh start
Check status
bash
sudo -u sonarqube /opt/sonarqube/bin/linux-x86-64/sonar.sh status
tail -f /opt/sonarqube/logs/sonar.log

```

Hurray !! Now you can access the `SonarQube Server` on `http://<ip-address>:9000` 


