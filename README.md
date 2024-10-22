# SAST with DVWA Project

## Project Overview

This project demonstrates performing **Static Application Security Testing (SAST)** using **SonarQube** on the **Damn Vulnerable Web Application (DVWA)** to identify security vulnerabilities and improve code quality. The DVWA is a PHP/MySQL web application that aims to be an educational tool for security professionals.

---

## Warning

Damn Vulnerable Web Application (DVWA) is a PHP/MySQL web application that is damn vulnerable. Its main goal is to be an aid for security professionals to test their skills and tools in a legal environment, help web developers better understand the processes of securing web applications and to aid both students & teachers to learn about web application security in a controlled class room environment.

The aim of DVWA is to practice some of the most common web vulnerabilities, with various levels of difficulty, with a simple straightforward interface. Please note, there are both documented and undocumented vulnerabilities with this software. This is intentional. You are encouraged to try and discover as many issues as possible.

---

## Disclaimer

This project is for educational purposes only. The authors are not responsible for any misuse of the information provided.

---

## Pre-requisites

   * Operating System: Debian-based system (Kali, Ubuntu, Kubuntu, Linux Mint, Zorin OS)
   * Docker
   * SonarQube
   * SonarQube Scanner
   * Java 17
   * Clone the project from [DVWA Project](https://pages.github.com/).

### Let's move to implemetation steps of this project.

---
## Project Implemetation

### 1. Install Docker

#### Open Your Terminal and run below:

```
sudo apt-get update

sudo apt-get install -y docker.io

sudo systemctl start docker

sudo systemctl enable docker
```
---
### 2. Install Git

```
sudo apt-get install -y git
```
---
### 3. Pull the SonarQube Latest Docker image

```
sudo docker pull sonarqube:latest
```
---
### 4. Run SonarQube

```
sudo docker run -d --name sonarqube -p 9000:9000 sonarqube:latest
```
---
### 5. Access SonarQube 

   * Open your browser and navigate to `http://localhost:9000`.

   * Log in with default credentials Username: `admin` and Password: `admin` change the password for further use.



   
> #### Note: SonarQube might take some time to load depending upon your resource usage.

#### Create New Project On SonarQube:

* Click on the "+" icon in the top-right corner of the dashboard.

   * Select "Create Project" from the dropdown menu.

   * Choose the project type (e.g., Local, GitHub, GitLab, etc.).

   * Enter the project name, project key, and other required details.

   * Click "Create" to create the project.

#### Generate A Token:

  * Go to User > My Account > Security.

  * Click on the "Generate" button next to "Tokens".

  * Select the token type (e.g., User, Project Analysis, Global Analysis).

  * Choose an expiration date for the token (optional).

  * Click "Generate" to create the token.

  * Copy the generated token value for further use.
---

### 6. Create a directory for your project

```
mkdir <your-directory>
```
#### Navigate to your directory
```
cd <your-directory> 
```
---
### 7. Clone the DVWA source-code

```
git clone https://github.com/yourusername/sast-dvwa-project.git
```

#### Navigate to the project directory:

```
cd your-directory/sast-dvwa-project
```
---
### 8. Install SonarQube Scanner

```
wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.6.2.2472-linux.zip
```
---
### 9. Unzip Downloaded File

```
unzip sonar-scanner-cli-4.6.2.2472-linux.zip

mv sonar-scanner-4.6.2.2472-linux/ sonar-scanner
```
---
### 10. Configure SonarQube Scanner

#### Open the configuration file:

```
nano ~/sast-dvwa-project/sonar-scanner/conf/sonar-scanner.properties
```

#### Add the following to the configuration file:

```
sonar.projectKey=<Your-Project-Key>
sonar.projectName=<Your-Project-Name>
sonar.projectVersion=1.0
sonar.sources=.
sonar.inclusions=**/*.php
sonar.language=php
sonar.sourceEncoding=UTF-8
sonar.host.url=http://localhost:9000
sonar.login=<your-sonarqube-token>
sonar.exclusions=**/config/**,**/docs/**,**/external/**,**/README*.md,**/*.txt
```

> #### Note: Replace the `ProjectKey`, `ProjectName` and `YourToken` from values generated from SonarQube Project.

#### Save & Exit:

* Press `Ctrl + X`, then `Y`, and `Enter` to save and close the file.

> #### Note: I see the script is setting up the environment variables. The issue seems to be `use_embedded_jre=true`, which forces SonarScanner to use its own JRE, ignoring the system settings. We need to disable this and force it to use `Java 17`. To fix this:

#### Navigate to the Directory: 
Open the sonar-scanner run file in a text editor. Run the following command:

```
nano ~/your-directory/sast-dvwa-project/sonar-scanner/bin/sonar-scanner
```
#### Make below changes to script:

1. Change `use_embedded_jre=true` to `use_embedded_jre=false`

2. Remove the entire block that sets `JAVA_HOME` to the embedded JRE:

#### Remove this block:
```
 if [ "$use_embedded_jre" = true ]; 
then
   export JAVA_HOME="$sonar_scanner_home/jre"
 fi
```
#### Here's how your script should look:

```
use_embedded_jre=false

if [ -n "$JAVA_HOME" ]
then
  java_cmd="$JAVA_HOME/bin/java"
else
  java_cmd="`which java`"
fi

if [ -z "$java_cmd" -o ! -x "$java_cmd" ] ; then
  echo "Could not find 'java' executable in JAVA_HOME or PATH."
  exit 1
fi

project_home=`pwd`

exec "$java_cmd" \
  -Djava.awt.headless=true \
  $SONAR_SCANNER_OPTS $SONAR_SCANNER_DEBUG_OPTS \
  -classpath  "$jar_file" \
  -Dscanner.home="$sonar_scanner_home" \
  -Dproject.home="$project_home" \
  org.sonarsource.scanner.cli.Main "$@"
```

#### Save & Exit:

 * Press `Ctrl + X`, then `Y`, and `Enter` to save and close the file.

---

### 11. Run the SonarQube Scanner

#### Navigate to your project directory:

```
cd your-directory/sast-dvwa-project
```

#### Run the SonarQube Scanner:

```
../sonar-scanner/bin/sonar-scanner
```

#### Monitor the Output:

* The scanner will show progress in the terminal. Look for a success message indicating that the analysis report has been uploaded to SonarQube.

---

### 12. Review the SonarQube Report

### Access SonarQube:

* Open your browser and navigate to `http://localhost:9000`.

* Log in and go to your project (`Your-Project-Name`).

* Review the analysis report for detected vulnerabilities.









