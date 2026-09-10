Software Engineering Lab Internal — Maven + Git + Docker Cheatsheet
Quick command/reference sheet based on the SET-1 and SET-2 exam patterns discussed above.

1. Git — Clone / Repository Setup
Clone repository
git clone https://github.com/<username>/<repo>.git
 
SSH:

git clone git@github.com:<username>/<repo>.git
 
Example:

git clone https://github.com/deepthisagar7/AI-OLMS.git
cd AI-OLMS
 
Check files:

ls
 
Check Maven project:

ls pom.xml
 
Initialize a new repository:

git init
git status
git add .
git commit -m "Initial commit"
 
Connect GitHub:

git remote add origin https://github.com/<username>/<repo>.git
git remote -v
git branch -M main
git push -u origin main
 
2. Git — Branches
Create and switch:

git switch -c feature/homepage
 
Example:

git switch -c feature/course-recommendation
 
Older equivalent:

git checkout -b feature/homepage
 
Check branches:

git branch
 
Switch branch:

git switch main
 
3. Git — Add / Commit / Status
git status
git add .
git commit -m "Add homepage and README"
 
Create README:

touch README.md
 
Check commit history:

git log --oneline
git log --oneline --graph --all
 
Correct the latest unpushed commit message:

git commit --amend -m "Added Assignment Module"
 
4. Git — Pull / Rebase / Merge
Get latest main changes without losing local commits:

git pull --rebase origin main
 
Rebase feature branch onto main:

git switch main
git pull origin main
git switch feature/homepage
git rebase main
 
Merge feature into main:

git switch main
git pull origin main
git merge feature/homepage
git push origin main
 
5. Git — Undo / Recover
Recover deleted uncommitted file:

git restore course.jsp
 
Unstage everything without deleting files:

git restore --staged .
 
Unstage one file:

git restore --staged <file>
 
Remove tracked file but keep it locally:

git rm --cached <file>
 
Undo a commit while keeping the commit in history:

git revert <commit-id>
 
Undo latest local commit while keeping changes staged:

git reset --soft HEAD~1
 
Amend latest commit:

git commit --amend
 
6. Git — Merge Conflict
Start merge:

git merge main
 
Identify conflict:

git status
 
Open the conflicted file and resolve:

<<<<<<< HEAD
your changes
=======
main branch changes
>>>>>>> main
 
Delete the conflict markers and keep the correct code.

Stage:

git add course.jsp
 
Complete merge:

git commit -m "Resolve merge conflict in course.jsp"
 
Push:

git push origin feature/course-recommendation
 
7. Git — Compare Branches
Code differences:

git diff main...feature/homepage
 
Commits in feature but not main:

git log main..feature/homepage
 
Commits in main but not feature:

git log feature/homepage..main
 
Visual history:

git log --oneline --graph --all
 
8. Git — .gitignore
Typical Maven/Eclipse .gitignore:

target/
.classpath
.project
.settings/
*.class
*.log
 
Create:

touch .gitignore
 
Add and commit:

git add .gitignore
git commit -m "Add gitignore"
 
If a file is already tracked:

git rm --cached <file>
 
Then add the file/pattern to .gitignore.

9. Maven — Check Java and Maven
Check installed Java:

java -version
 
Check Java compiler:

javac -version
 
Check Maven and the Java Maven is using:

mvn -version
 
Check JAVA_HOME:

echo $JAVA_HOME
 
Windows Git Bash example for Java 17:

export JAVA_HOME="/c/Program Files/Java/jdk-17"
export PATH="$JAVA_HOME/bin:$PATH"
 
Verify:

java -version
mvn -version
 
10. Maven — Create a JAR Project
Generate a Maven project:

mvn archetype:generate -DgroupId=com.example -DartifactId=myapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
 
Enter:

cd myapp
 
Build:

mvn clean package
 
Output:

target/myapp-1.0-SNAPSHOT.jar
 
JAR packaging:

<packaging>jar</packaging>
 
jar is Maven’s default packaging.

11. Maven — WAR Project
For a web application:

<packaging>war</packaging>
 
Build:

mvn clean package
 
Output:

target/AI-OLMS.war
 
12. Maven — Java 17 WAR pom.xml
Relevant corrected structure:

<project>
    <modelVersion>4.0.0</modelVersion>
 
    <groupId>com.example</groupId>
    <artifactId>AI-OLMS</artifactId>
    <version>1.0-SNAPSHOT</version>
 
    <packaging>war</packaging>
 
    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
 
    <build>
        <finalName>AI-OLMS</finalName>
 
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.13.0</version>
                <configuration>
                    <source>17</source>
                    <target>17</target>
                </configuration>
            </plugin>
 
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>3.4.0</version>
            </plugin>
        </plugins>
    </build>
</project>
 
13. Maven — Executable JAR
Change:

<packaging>war</packaging>
 
to:

<packaging>jar</packaging>
 
Configure maven-jar-plugin:

<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <configuration>
        <archive>
            <manifest>
                <mainClass>com.example.App</mainClass>
            </manifest>
        </archive>
    </configuration>
</plugin>
 
Main class must contain:

public static void main(String[] args) {
    // application code
}
 
Build:

mvn clean package
 
14. Maven — Lifecycle
clean   -> deletes target/
compile -> compiles source code
test    -> runs tests
package -> creates JAR/WAR
install -> installs package into local .m2 repository
 
WAR generation:

mvn package
 
Clean + build:

mvn clean package
 
Detailed debugging:

mvn clean package -X
 
15. Maven — Dependencies
Show dependency tree:

mvn dependency:tree
 
Search dependency:

mvn dependency:tree | grep <dependency-name>
 
Check local Maven repository:

ls ~/.m2/repository
 
Find JARs:

find ~/.m2/repository -name "*.jar"
 
Find a particular library:

find ~/.m2/repository -name "*library-name*.jar"
 
Maven normally resolves conflicting dependency versions using dependency mediation; the nearest dependency in the dependency tree generally wins.

16. Maven — JUnit / Tests
Run tests:

mvn test
 
Compiled test classes:

target/test-classes/
 
JUnit/Surefire reports:

target/surefire-reports/
 
Run one test class:

mvn -Dtest=AppTest test
 
Rerun failing tests:

mvn test -Dsurefire.rerunFailingTestsCount=1
 
17. Maven — Java Version Failure
Check:

java -version
javac -version
mvn -version
 
If Maven is using the wrong JDK, fix JAVA_HOME and PATH, then verify:

mvn -version
 
Java 17 compiler configuration:

<properties>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
</properties>
 
Then:

mvn clean package
 
18. Maven — Require Java 17
Add Maven Enforcer Plugin:

<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-enforcer-plugin</artifactId>
    <version>3.5.0</version>
    <executions>
        <execution>
            <id>enforce-java</id>
            <goals>
                <goal>enforce</goal>
            </goals>
            <configuration>
                <rules>
                    <requireJavaVersion>
                        <version>17</version>
                    </requireJavaVersion>
                </rules>
            </configuration>
        </execution>
    </executions>
</plugin>
 
19. Docker — Basic Commands
Check Docker:

docker --version
docker info
 
List images:

docker images
 
List running containers:

docker ps
 
List all containers:

docker ps -a
 
Build image:

docker build -t ai-olms:latest .
 
Run container:

docker run -d -p 7012:8080 ai-olms:latest
 
Stop:

docker stop <container_id>
 
Start:

docker start <container_id>
 
Restart:

docker restart <container_id>
 
Remove:

docker rm <container_id>
 
Force remove:

docker rm -f <container_id>
 
Remove image:

docker rmi <image>
 
20. Docker — Troubleshooting
Logs:

docker logs <container_id>
 
Follow logs:

docker logs -f <container_id>
 
Inspect:

docker inspect <container_id>
 
Check port mapping:

docker port <container_id>
 
Enter container:

docker exec -it <container_id> bash
 
Check Tomcat webapps:

docker exec -it <container_id> ls /usr/local/tomcat/webapps
 
21. Dockerfile — Maven + Java 17 + Tomcat
For a Maven WAR application:

# Stage 1: Build
FROM maven:3.9-eclipse-temurin-17 AS build
 
WORKDIR /app
 
COPY pom.xml .
COPY src ./src
 
RUN mvn clean package -DskipTests
 
# Stage 2: Runtime
FROM tomcat:10.1-jdk17
 
RUN rm -rf /usr/local/tomcat/webapps/*
 
COPY --from=build /app/target/*.war /usr/local/tomcat/webapps/
 
EXPOSE 8080
 
CMD ["catalina.sh", "run"]
 
Build:

docker build -t ai-olms:latest .
 
Run:

docker run -d -p 7012:8080 ai-olms:latest
 
Check:

docker ps
docker logs <container_id>
 
22. Docker — Tomcat Standalone
Pull Tomcat:

docker pull tomcat:10.1-jdk17
 
Run Tomcat:

docker run -d --name tomcat-server -p 7070:8080 tomcat:10.1-jdk17
 
Verify:

docker ps
 
Copy WAR:

docker cp target/AI-OLMS.war tomcat-server:/usr/local/tomcat/webapps/
 
Verify WAR:

docker exec -it tomcat-server ls /usr/local/tomcat/webapps
 
Check logs:

docker logs tomcat-server
 
Open:

http://localhost:7070/AI-OLMS
 
If WAR is named ROOT.war:

http://localhost:7070
 
23. Docker — 404 Troubleshooting
Check deployed files:

docker exec -it <container_id> ls -l /usr/local/tomcat/webapps
 
Check extracted application:

docker exec -it <container_id> ls /usr/local/tomcat/webapps/AI-OLMS
 
Check logs:

docker logs <container_id>
 
Possible causes:

1. Wrong WAR context path
2. WAR not copied into webapps
3. WAR deployment failed
4. Application/web configuration problem
 
For:

AI-OLMS.war
 
try:

http://localhost:7012/AI-OLMS
 
24. Docker — Ubuntu + Python
Pull Ubuntu:

docker pull ubuntu
 
Run:

docker run -dit --name python-container ubuntu
 
Enter:

docker exec -it python-container bash
 
Inside container:

apt update
apt install -y python3
python3 --version
 
Run Python:

python3
 
Example:

print("Hello from Docker")
 
Exit:

exit()
 
Then:

exit
 
25. Docker Hub — Push Image
Login:

docker login
 
Tag:

docker tag ai-olms:latest <dockerhub-username>/ai-olms:latest
 
Example:

docker tag ai-olms:latest sreearnav/ai-olms:latest
 
Push:

docker push sreearnav/ai-olms:latest
 
For OLES image:

docker tag oles:latest sreearnav/oles:latest
docker push sreearnav/oles:latest
 
Verify locally:

docker images
 
Then verify the repository on Docker Hub.

26. Docker — Cleanup
Remove unused containers/images:

docker system prune
 
More aggressive:

docker system prune -a
 
27. Full Exam Workflow — Maven WAR + Docker
# Clone
git clone https://github.com/<username>/<repo>.git
cd <repo>
 
# Check Java/Maven
java -version
mvn -version
 
# Build WAR
mvn clean package
 
# Check WAR
ls target/
 
# Build Docker image
docker build -t ai-olms:latest .
 
# Run
docker run -d -p 7012:8080 ai-olms:latest
 
# Verify
docker ps
docker logs <container_id>
docker exec -it <container_id> ls /usr/local/tomcat/webapps
 
# Test
# http://localhost:7012/AI-OLMS
 
# Docker Hub
docker login
docker tag ai-olms:latest <username>/ai-olms:latest
docker push <username>/ai-olms:latest
 
28. Full Exam Workflow — Git
git clone <ssh-or-https-url>
cd <repo>
 
git status
git remote -v
 
git switch -c feature/homepage
 
touch README.md
git add .
git commit -m "Add homepage and README"
 
git pull --rebase origin main
 
git switch main
git pull origin main
git switch feature/homepage
git rebase main
 
git diff main...feature/homepage
git log main..feature/homepage
 
git switch main
git merge feature/homepage
git push origin main
 
git status
 
29. Important One-Line Answers
Question Answer

Maven Java compiler plugin maven-compiler-plugin Maven Java version mvn -version Maven debug mvn clean package -X Build project mvn clean package WAR packaging <packaging>war</packaging> JAR packaging <packaging>jar</packaging> WAR generated by package phase Test classes target/test-classes/ JUnit reports target/surefire-reports/ Dependency tree mvn dependency:tree New branch git switch -c <branch> Undo pushed commit git revert <commit> Recover deleted file git restore <file> Unstage files git restore --staged . Keep file locally, stop tracking git rm --cached <file> Reorganize feature with main git rebase main Merge feature git merge feature/homepage Docker build docker build -t <name>:<tag> . Docker run docker run -d -p 7070:8080 <image> Docker logs docker logs <container> Enter container docker exec -it <container> bash Push Docker image docker push <username>/<repo>:<tag>
