# Devops

Q1. (15 Marks) 
Create a new Git repository locally, add a Java file (HelloWorld.java), commit it, and push it to 
your GitHub repository. 
Expected Output: Code and commit visible on GitHub.

# Quick plan (what you'll do)

1. Create a GitHub repository on the web.
2. On your college PC create a local folder, add `HelloWorld.java`.
3. Initialize Git, make your first commit.
4. Add the GitHub repo as a remote and push the commit.
5. Verify code/commit on GitHub.


# 0) Prerequisites

Make sure:

* Git is installed. Check with:

```bash
git --version
```

* You have a GitHub account and can log in from the browser.
* If on Windows: use Git Bash or PowerShell (Git Bash is easiest).
* If your college PC blocks SSH, use HTTPS.

---

# 1) Create the repository on GitHub (in your browser)

1. Log into github.com.
2. Click **+** → **New repository**.
3. Fill:

   * Repository name: e.g. `hello-java`
   * Description: optional
   * **Public** or **Private** as required by your instructor
   * **Do not** initialize with README (we will push an existing repo). (If you do initialize it, commands differ slightly — see note below.)
4. Click **Create repository**.

After creation GitHub will show you the repo page and give you commands for pushing an existing repo — keep that page open.

---

# 2) Create project folder and HelloWorld.java (on the college PC)

Open terminal (Git Bash / Linux terminal / macOS Terminal / PowerShell):

```bash
# choose a folder, e.g. your home or Desktop
cd ~
mkdir hello-java
cd hello-java
```

Create `HelloWorld.java` (you can use `nano`, `vim`, or a text editor):

```bash
cat > HelloWorld.java <<'EOF'
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, world!");
    }
}
EOF
```

Check file:

```bash
ls -l
cat HelloWorld.java
```

You should see the file content printed.

---

# 3) Initialize Git locally, set user (one-time per machine)

If you haven’t set your name/email on this machine:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Initialize repo and create first commit:

```bash
git init
git add HelloWorld.java
git status              # shows staged file
git commit -m "Add HelloWorld.java"
```

Expected `git commit` output: something like

```
[master (root-commit) a1b2c3d] Add HelloWorld.java
 1 file changed, 10 insertions(+)
 create mode 100644 HelloWorld.java
```

> Note: Git may name the default branch `master`. Some systems use `main`. We'll ensure branch is `main` below (recommended).

Make branch `main` (recommended):

```bash
git branch -M main
```

---

# 4A) Push via HTTPS (if you prefer username/password or Personal Access Token)

On the GitHub repo page you copied earlier, GitHub shows commands such as:

```bash
# example (replace the URL with yours)
git remote add origin https://github.com/<your-username>/hello-java.git
git push -u origin main
```

Run those commands:

```bash
git remote add origin https://github.com/<your-username>/hello-java.git
git push -u origin main
```

**Authentication:**

* If asked for username/password:

  * If you do **NOT** have 2FA: GitHub will accept your username and password.
  * If you **do** have 2FA (two-factor), you must use a **Personal Access Token (PAT)** instead of your password. Create it in GitHub Settings → Developer settings → Personal access tokens → Tokens (classic) or fine-grained tokens. Give `repo` scope for classic token (or appropriate access for fine-grained).
* When prompted for password, paste the token (it will look like a long string). On Git Bash, username is your GitHub username; password is the token.

If push succeeds you’ll see:

```
Counting objects: 3, done.
Writing objects: 100% (3/3),  ... 
To https://github.com/...
 * [new branch]      main -> main
Branch 'main' set up to track 'origin/main'.
```

---

# 4B) Push via SSH (recommended if you’ll push often)

If you prefer SSH (one-time setup), do:

## Generate SSH key (if you don't have one)

```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
# If asked for a filename, accept default. Enter a passphrase or leave empty.
```

If system doesn't support ed25519, use rsa:

```bash
ssh-keygen -t rsa -b 4096 -C "your.email@example.com"
```

## Add SSH key to ssh-agent

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519    # or ~/.ssh/id_rsa
```

## Copy public key and add to GitHub

```bash
# print it
cat ~/.ssh/id_ed25519.pub
```

Copy entire output, then in GitHub go to **Settings → SSH and GPG keys → New SSH key**, paste and save.

## Use SSH remote and push

On local repo:

```bash
git remote add origin git@github.com:<your-username>/hello-java.git
git push -u origin main
```

If SSH works, you'll see something like:

```
Enumerating objects: 3, done.
Writing objects: 100% (3/3),  ... 
To github.com:your-username/hello-java.git
 * [new branch]      main -> main
Branch 'main' set up to track 'origin/main'.
```

If you see `Permission denied (publickey)`, check the SSH key steps: ensure the public key was added to GitHub and the ssh-agent has the private key loaded.

---

# 5) If the GitHub repo was created *with* a README (you see `remote: error: failed to push some refs`):

This means the remote has commits and you must pull first or force push. Recommended safe way:

```bash
# fetch remote and merge
git pull --rebase origin main
# resolve any conflicts (should be none if only README exists)
git push -u origin main
```

If you are certain remote has only README and you want to overwrite, you could do:

```bash
git push -u origin main --force
```

(Only do `--force` if you understand it rewrites remote history.)

---

# 6) Verify on GitHub

Open your repository page `https://github.com/<your-username>/hello-java`
You should see `HelloWorld.java` and the commit message visible under **Commits** or file view.

---

# 7) Extra: compile & run the Java file locally (optional)

If Java is installed:

```bash
javac HelloWorld.java
java HelloWorld
```

Output should be:

```
Hello, world!
```

---
Q2. (15 Marks) 
Create a simple Maven project using the command line and build it using mvn clean package. 
Expected Output: Successful build with .jar file in target folder.

# Troubleshooting cheatsheet

* `git: command not found` → install Git (on Windows install Git for Windows; on Ubuntu `sudo apt install git`).
* `Permission denied (publickey)` → SSH key not added or ssh-agent not running. Re-run key setup and add key to GitHub.
* `remote: Invalid username or password` → using password with 2FA — use PAT instead.
* `error: failed to push some refs` → remote has commits; run `git pull --rebase origin main` then push.

---

# Full example session (copy-paste)

Replace `<your-username>` with your GitHub username.

```bash
# create folder & file
mkdir ~/hello-java && cd ~/hello-java
cat > HelloWorld.java <<'EOF'
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, world!");
    }
}
EOF

# git setup & commit
git init
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git add HelloWorld.java
git commit -m "Add HelloWorld.java"
git branch -M main

# add remote and push (HTTPS)
git remote add origin https://github.com/<your-username>/hello-java.git
git push -u origin main
```

Or (if using SSH, after creating SSH key and adding to GitHub):

```bash
git remote add origin git@github.com:<your-username>/hello-java.git
git push -u origin main
```

---

# Final checklist to hand in / show your professor

* [ ] GitHub repo link (e.g. `https://github.com/<your-username>/hello-java`)
* [ ] `HelloWorld.java` visible in repo
* [ ] Commit message visible (e.g., `Add HelloWorld.java`)
* [ ] (Optional) Screenshot of terminal showing `git commit` and `git push` output

---

Perfect 👍 — this is another **hands-on practical DevOps/Java build question**.
Let’s go **step-by-step** so you can perform it easily on your college PC and get the **expected output: successful .jar file inside the `target` folder**.

---

## ✅ **Q2. Create a simple Maven project using the command line and build it using `mvn clean package`**

---

## 🧩 **Goal**

You will:

1. Create a new Maven project from the terminal.
2. Add a simple Java program.
3. Build the project using Maven.
4. Confirm that a `.jar` file is created inside the `target` folder.

---

## 🧰 **Pre-requisites**

Make sure:

* Java and Maven are installed.

Check versions:

```bash
java -version
mvn -version
```

✅ If both commands show version info → you’re good.
❌ If not, ask the lab instructor to install them or set `JAVA_HOME` / `MAVEN_HOME`.

---

## 🪜 **Step-by-Step Procedure**

### **Step 1: Open Terminal (or Git Bash / CMD)**

Choose a working directory, e.g. Desktop or home folder:

```bash
cd ~/Desktop
```

---

### **Step 2: Create a simple Maven project using the archetype command**

Run this command:

```bash
mvn archetype:generate -DgroupId=com.example \
-DartifactId=MyApp \
-DarchetypeArtifactId=maven-archetype-quickstart \
-DinteractiveMode=false
```

**Explanation:**

* `groupId`: your project’s package name (like a company domain)
* `artifactId`: project name
* `maven-archetype-quickstart`: template for basic Java app
* `-DinteractiveMode=false`: runs automatically (no prompts)

---

### **Step 3: Go into your new project**

```bash
cd MyApp
```

Inside, you’ll have this structure:

```
MyApp/
 ├── pom.xml
 └── src/
     ├── main/java/com/example/App.java
     └── test/java/com/example/AppTest.java
```

---

### **Step 4: Open and check your Java code**

You can check the main Java file:

```bash
cat src/main/java/com/example/App.java
```

You’ll see something like:

```java
package com.example;

public class App {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

You can modify it if needed — for example:

```java
System.out.println("Hello from Maven project!");
```

---

### **Step 5: Build the project**

Now build using Maven commands:

#### a) Clean old builds

```bash
mvn clean
```

#### b) Package (compile + test + create jar)

```bash
mvn package
```

✅ **Expected Output:**
At the end, you should see:

```
[INFO] BUILD SUCCESS
```

and inside the `target` folder:

```
target/MyApp-1.0-SNAPSHOT.jar
```

---

### **Step 6: Verify the `.jar` file**

List the target folder:

```bash
ls target
```

Expected files:

```
classes/
test-classes/
MyApp-1.0-SNAPSHOT.jar
```

---

### **Step 7: Run the jar (optional)**

You can test it using Java:

```bash
java -cp target/MyApp-1.0-SNAPSHOT.jar com.example.App
```

✅ Output:

```
Hello from Maven project!
```

---

## 🧾 **Expected Output (for submission / screenshot)**

When you run:

```bash
mvn clean package
```

You should capture output similar to:

```
[INFO] Scanning for projects...
[INFO] Building MyApp 1.0-SNAPSHOT
[INFO] Compiling 1 source file to /MyApp/target/classes
[INFO] BUILD SUCCESS
```

and `target/MyApp-1.0-SNAPSHOT.jar` must be visible.

---

## 🧠 **Concept Behind the Question**

* **Maven** is a build automation tool for Java.
* **`pom.xml`** is the project descriptor.
* **`mvn clean`** → removes old build files.
* **`mvn package`** → compiles source + runs tests + packages output as a `.jar`.

---

Q3. (10 Marks) 
Create a Dockerfile for your Maven project and build a Docker image using the command line. 
Expected Output: Docker image visible in docker images. 
Perfect! This is the **Docker hands-on question** — we’ll take your Maven project from **Q2** and containerize it step by step. I’ll make it **very detailed** so you can perform it on your college PC.

---

# ✅ **Q3. Dockerize Maven Project**

**Goal:**

* Create a Dockerfile for the Maven project (`MyApp`)
* Build a Docker image
* Verify the image is created with `docker images`

---

## 🧰 **Prerequisites**

1. Docker installed: Check:

```bash
docker --version
```

2. Your Maven project is ready (`MyApp`) and built successfully (`target/MyApp-1.0-SNAPSHOT.jar`)
3. Terminal or PowerShell access

---

## 🪜 **Step 1: Go to your project folder**

```bash
cd ~/Desktop/MyApp
```

---

## 🪜 **Step 2: Create a Dockerfile**

Create a file named `Dockerfile` (no extension):

```bash
touch Dockerfile
```

Open it in an editor (`nano Dockerfile`) or directly write:

```bash
cat > Dockerfile <<'EOF'
# Step 1: Use official OpenJDK image
FROM openjdk:17-jdk-slim

# Step 2: Set working directory inside container
WORKDIR /app

# Step 3: Copy the jar file from target folder to container
COPY target/MyApp-1.0-SNAPSHOT.jar app.jar

# Step 4: Command to run the jar
ENTRYPOINT ["java","-jar","app.jar"]
EOF
```

**Explanation of Dockerfile:**

| Step | Command                                      | Purpose                           |
| ---- | -------------------------------------------- | --------------------------------- |
| 1    | `FROM openjdk:17-jdk-slim`                   | Base image with Java 17           |
| 2    | `WORKDIR /app`                               | Directory inside container        |
| 3    | `COPY target/MyApp-1.0-SNAPSHOT.jar app.jar` | Copy your built jar               |
| 4    | `ENTRYPOINT ["java","-jar","app.jar"]`       | Run the jar when container starts |

---

## 🪜 **Step 3: Build the Docker image**

Run:

```bash
docker build -t myapp-image .
```

**Explanation:**

* `-t myapp-image` → tag name for the image
* `.` → current directory (Dockerfile location)

✅ Expected Output (partial):

```
Sending build context to Docker daemon  5.12MB
Step 1/4 : FROM openjdk:17-jdk-slim
 ---> ...
Step 2/4 : WORKDIR /app
 ---> Using cache
Step 3/4 : COPY target/MyApp-1.0-SNAPSHOT.jar app.jar
 ---> ...
Step 4/4 : ENTRYPOINT ["java","-jar","app.jar"]
 ---> ...
Successfully built abcdef123456
Successfully tagged myapp-image:latest
```

---

## 🪜 **Step 4: Verify the Docker image**

```bash
docker images
```

You should see:

```
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
myapp-image     latest    abcdef123456   10 seconds ago   120MB
```

---

## 🪜 **Step 5: Run the Docker container (optional test)**

```bash
docker run --rm myapp-image
```

Expected output:

```
Hello from Maven project!
```

> `--rm` removes the container after it stops

---

## 🧠 **Concept Behind This Question**

* Docker allows you to **package your Java app + runtime** in a container.
* `Dockerfile` defines how to build the image: base image → copy app → run app.
* `docker build` → creates an image
* `docker images` → lists all local images
* `docker run` → runs the app inside a container

---
Set 2 
Q1. (15 Marks) 
Create a new Jenkins Freestyle project that pulls code from your GitHub repository and builds it 
using Maven (clean package). 
Expected Output: Jenkins job builds the project successfully. 

Great! This is a **Jenkins CI/CD practical question**. I’ll give you a **step-by-step guide** to create a Freestyle project that pulls your Maven project from GitHub and builds it using `mvn clean package`.

---

# ✅ **Q1 (Set 2): Jenkins Freestyle Project for Maven Build**

**Goal:**

* Create a Jenkins Freestyle job
* Pull code from GitHub
* Build it with Maven (`clean package`)
* Confirm build success

---

## 🧰 **Prerequisites**

1. Jenkins installed and running (locally or on college server). Check:

```
http://localhost:8080
```

2. Jenkins has **Maven installed**:

   * Go to **Manage Jenkins → Global Tool Configuration → Maven**
   * Add a Maven installation or check "Install automatically"
3. Jenkins has **Git plugin installed** (default in recent versions).
4. GitHub repository created in **Q1 or Q2**, with a Maven project.

---

## 🪜 **Step 1: Log in to Jenkins**

* Open browser: `http://localhost:8080`
* Enter username/password
* Dashboard appears

---

## 🪜 **Step 2: Create a Freestyle Project**

1. Click **New Item** (top left)
2. Enter name: `MyApp-Build`
3. Select **Freestyle project**
4. Click **OK**

---

## 🪜 **Step 3: Configure GitHub Repository**

1. Scroll to **Source Code Management → Git**
2. Repository URL:

   * HTTPS: `https://github.com/<username>/MyApp.git`
   * SSH: `git@github.com:<username>/MyApp.git`
3. If using HTTPS: enter GitHub **username** and **Personal Access Token** (PAT) in credentials.
4. Branch to build: `main` (or `master` if your repo uses it)

---

## 🪜 **Step 4: Add Build Step**

1. Scroll to **Build → Add build step → Invoke top-level Maven targets**
2. Set:

   * **Goals:** `clean package`
   * **POM:** `pom.xml` (default if in root folder)
3. Leave other fields default.

---

## 🪜 **Step 5: Save and Build**

1. Click **Save**
2. Click **Build Now** (left menu)

---

## 🪜 **Step 6: Verify Build**

1. Console output: Click **#1 (Build Number) → Console Output**
2. Look for:

```
[INFO] Scanning for projects...
[INFO] Building MyApp 1.0-SNAPSHOT
[INFO] BUILD SUCCESS
```

3. Build should complete **successfully**

---

## 🪜 **Step 7: Optional Post-build Steps**

* **Archive the artifacts**:

  * Post-build action → **Archive the artifacts**
  * Enter: `target/*.jar`
  * This allows you to download the `.jar` from Jenkins.

---

## 🧠 **Concept Behind This Question**

* Jenkins is a **Continuous Integration tool**
* Freestyle project is the simplest Jenkins job type
* **Git** plugin pulls the code
* **Maven build step** compiles, tests, and packages the Java app
* Successful build shows Jenkins can automate the Maven build process

---
Q2. (15 Marks) 
Configure Jenkins to automatically trigger a build whenever new code is pushed to GitHub using 
a webhook. 
Expected Output: Jenkins job triggers automatically on every commit.
Perfect! This is the **GitHub webhook CI/CD automation question**. I’ll guide you step by step to **configure Jenkins so it triggers a build automatically whenever code is pushed to GitHub**.

---

# ✅ **Q2 (Set 2): Jenkins Automatic Build Trigger via GitHub Webhook**

**Goal:**

* Configure Jenkins to poll GitHub for changes using a webhook
* Whenever a commit is pushed to GitHub, Jenkins automatically builds the job

---

## 🧰 **Prerequisites**

1. Jenkins running and accessible from a browser

   * If local: `http://localhost:8080`
   * If on college network: Jenkins should be accessible externally or via local tunnel (e.g., **ngrok**)
2. Jenkins job exists (`MyApp-Build`)
3. GitHub repository exists and contains your project
4. GitHub plugin installed in Jenkins

---

## 🪜 **Step 1: Configure Jenkins Job to Build Remotely**

1. Go to your Jenkins job → **Configure**
2. Scroll to **Build Triggers** → select **GitHub hook trigger for GITScm polling**

   * This allows Jenkins to listen for GitHub webhook events

---

## 🪜 **Step 2: Make Jenkins Accessible to GitHub**

* GitHub needs to reach Jenkins over the internet. If Jenkins is local (localhost), GitHub cannot access it. Use **ngrok** to create a public URL:

```bash
# start ngrok on port 8080 (where Jenkins runs)
ngrok http 8080
```

* Copy the **Forwarding URL** (e.g., `https://abcd1234.ngrok.io`)

---

## 🪜 **Step 3: Configure GitHub Webhook**

1. Go to your GitHub repository → **Settings → Webhooks → Add webhook**
2. Fill in:

   * **Payload URL:** `https://<your-ngrok-id>.ngrok.io/github-webhook/`

     * `/github-webhook/` is required for the GitHub plugin in Jenkins
   * **Content type:** `application/json`
   * **Which events would you like to trigger this webhook?** → Select **Just the push event**
3. Click **Add webhook**

✅ GitHub will send a test ping → Jenkins should receive it (check Jenkins logs: **Manage Jenkins → System Log**)

---

## 🪜 **Step 4: Save Jenkins Job Configuration**

* Click **Save** in Jenkins
* Ensure **Build Triggers → GitHub hook trigger** is checked

---

## 🪜 **Step 5: Test the Automatic Trigger**

1. Make a small change in your project (e.g., update `HelloWorld.java`)
2. Commit and push to GitHub:

```bash
git add HelloWorld.java
git commit -m "Test automatic build trigger"
git push origin main
```

3. Go to Jenkins → Job `MyApp-Build`
4. You should see a **new build triggered automatically**
5. Console output should show Maven build:

```
[INFO] BUILD SUCCESS
```

---

## 🧠 **Concept Behind This Question**

* **GitHub Webhook:** Sends an HTTP POST to Jenkins whenever an event happens (push in this case)
* **Jenkins GitHub Plugin:** Listens for webhook events and triggers builds
* **CI/CD Benefit:** Builds happen automatically on every commit → fast feedback loop

---
Q3. (10 Marks) 
Run a container from a Docker image and verify it using docker ps and container logs. 
Expected Output: Container running successfully with correct output. 
Perfect! This question is about **running your Dockerized Maven project (from Q3 Set 1)** and verifying that it works. I’ll give you a **step-by-step detailed answer**.

---

# ✅ **Q3 (Set 2): Run Docker Container and Verify**

**Goal:**

* Run a container from your Docker image (`myapp-image`)
* Verify it is running with `docker ps`
* Check container logs to confirm output

---

## 🧰 **Prerequisites**

1. Docker installed and running:

```bash
docker --version
```

2. Docker image already built from Q3 Set 1 (`myapp-image`)
3. Terminal access

---

## 🪜 **Step 1: Run the Docker Container**

```bash
docker run -d --name myapp-container myapp-image
```

**Explanation:**

| Option                   | Purpose                                        |
| ------------------------ | ---------------------------------------------- |
| `-d`                     | Run container in detached mode (in background) |
| `--name myapp-container` | Give a friendly name to the container          |
| `myapp-image`            | Docker image name to run                       |

✅ Output: Docker will return a long **container ID**, e.g.,

```
a1b2c3d4e5f6g7h8i9j0
```

---

## 🪜 **Step 2: Verify Container is Running**

```bash
docker ps
```

Expected output:

```
CONTAINER ID   IMAGE          COMMAND                  STATUS          PORTS   NAMES
a1b2c3d4e5f6   myapp-image    "java -jar app.jar"      Up 5 seconds           myapp-container
```

* **STATUS:** should show `Up` → container is running
* **NAMES:** should match the name you gave (`myapp-container`)

---

## 🪜 **Step 3: Check Container Logs**

```bash
docker logs myapp-container
```

Expected output (from your Java program):

```
Hello from Maven project!
```

* This confirms that the Java program inside the container is running correctly.

---

## 🪜 **Step 4: Stop and Remove Container (optional)**

After verification:

```bash
docker stop myapp-container
docker rm myapp-container
```

* Stops and deletes the container to free resources

---

## 🧠 **Concept Behind This Question**

* `docker run` → launches a container from an image
* `docker ps` → lists all running containers
* `docker logs` → shows stdout/stderr logs from the container
* Ensures that the Docker image works correctly in an isolated environment

---

Set 3 
Q1. (15 Marks) 
Clone your GitHub repository, modify one Java file, commit and push the changes. 
Expected Output: Updated code with new commit visible on GitHub. 

Perfect! This is a **basic Git workflow question**. I’ll give you a **detailed step-by-step procedure** so you can perform it easily on your college PC.

---

# ✅ **Q1 (Set 3): Clone, Modify, Commit, and Push**

**Goal:**

* Clone your GitHub repo
* Modify a Java file
* Commit the changes
* Push to GitHub
* Confirm the new commit appears on GitHub

---

## 🧰 **Prerequisites**

1. Git installed on your PC:

```bash
git --version
```

2. GitHub repository exists (e.g., `MyApp`)
3. Terminal / Git Bash / PowerShell access

---

## 🪜 **Step 1: Clone the GitHub Repository**

Go to the folder where you want to store the repo:

```bash
cd ~/Desktop
```

Clone using HTTPS or SSH:

**HTTPS:**

```bash
git clone https://github.com/<username>/MyApp.git
```

**SSH:**

```bash
git clone git@github.com:<username>/MyApp.git
```

* Replace `<username>` with your GitHub username.
* After cloning, a folder `MyApp` is created.

Go into the project folder:

```bash
cd MyApp
```

Check files:

```bash
ls
```

You should see `pom.xml`, `src/`, etc.

---

## 🪜 **Step 2: Modify a Java File**

Open a Java file (e.g., `HelloWorld.java`) for editing:

```bash
nano src/main/java/com/example/App.java
```

Add a new line or change the print message:

```java
System.out.println("This is a modified version!");
```

Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X` in nano).

---

## 🪜 **Step 3: Stage the Changes**

Check the status:

```bash
git status
```

You should see something like:

```
modified: src/main/java/com/example/App.java
```

Stage the changes:

```bash
git add src/main/java/com/example/App.java
```

---

## 🪜 **Step 4: Commit the Changes**

```bash
git commit -m "Modify App.java to print new message"
```

Expected output:

```
[main abc1234] Modify App.java to print new message
 1 file changed, 1 insertion(+), 1 deletion(-)
```

---

## 🪜 **Step 5: Push Changes to GitHub**

```bash
git push origin main
```

* If using HTTPS with 2FA: enter **Personal Access Token** as password.
* If SSH: push works automatically if SSH key is configured.

Expected output:

```
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), done.
Total 3 (delta 1), reused 0 (delta 0)
To https://github.com/<username>/MyApp.git
   abc1234..def5678  main -> main
```

---

## 🪜 **Step 6: Verify on GitHub**

1. Go to your GitHub repo in a browser:
   `https://github.com/<username>/MyApp`
2. Check the file (`App.java`) → your modification should be visible
3. Click **Commits** → you should see your new commit message:
   `"Modify App.java to print new message"`

---

## 🧠 **Concept Behind This Question**

* **Clone:** copies GitHub repo to local machine
* **Modify + Stage:** `git add` prepares files for commit
* **Commit:** creates a snapshot of changes with a message
* **Push:** uploads commits to GitHub

This demonstrates the basic **Git workflow for collaboration and version control**.

---
Q2. (15 Marks) 
Create a Jenkins pipeline with stages: Checkout → Build (Maven) → Test. Execute the pipeline 
and show console output. 
Expected Output: Pipeline stages execute successfully.

Perfect! This is a **Jenkins Pipeline (Declarative) question**. I’ll give you a **step-by-step guide**, including the **Jenkinsfile** you can use for your Maven project.

---

# ✅ **Q2 (Set 3): Jenkins Pipeline with Checkout → Build → Test**

**Goal:**

* Create a Jenkins Pipeline job
* Pipeline stages:

  1. **Checkout** – pull code from GitHub
  2. **Build** – run `mvn clean package`
  3. **Test** – run unit tests
* Execute pipeline and view console output

---

## 🧰 **Prerequisites**

1. Jenkins installed and running
2. GitHub repo exists with Maven project
3. Maven installed in Jenkins (Global Tool Configuration)
4. Git plugin installed

---

## 🪜 **Step 1: Create a Jenkins Pipeline Job**

1. Open Jenkins → click **New Item**
2. Enter job name: `MyApp-Pipeline`
3. Select **Pipeline**
4. Click **OK**

---

## 🪜 **Step 2: Configure Pipeline Script**

Scroll to **Pipeline → Definition → Pipeline script** and enter the following **Declarative Jenkinsfile**:

```groovy
pipeline {
    agent any

    tools {
        maven 'Maven3' // Name of Maven installation in Jenkins
        jdk 'Java17'   // Name of JDK installation in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/<username>/MyApp.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
```

**Notes:**

* Replace `<username>` with your GitHub username
* `Maven3` and `Java17` should match your Jenkins **Global Tool Configuration** names
* `sh` is for Linux/Mac. On Windows, you can replace with `bat`:

```groovy
bat 'mvn clean package'
bat 'mvn test'
```

---

## 🪜 **Step 3: Save and Run the Pipeline**

1. Click **Save**
2. Click **Build Now**

---

## 🪜 **Step 4: View Console Output**

1. Click **#1 (Build Number) → Console Output**
2. You should see:

```
[Pipeline] stage
[Pipeline] { (Checkout)
Cloning repository https://github.com/<username>/MyApp.git
...
[Pipeline] } // stage
[Pipeline] stage
[Pipeline] { (Build)
[INFO] Scanning for projects...
[INFO] BUILD SUCCESS
...
[Pipeline] } // stage
[Pipeline] stage
[Pipeline] { (Test)
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
...
[Pipeline] } // stage
[Pipeline] echo
Pipeline executed successfully!
```

* Each stage executes sequentially: **Checkout → Build → Test**
* `BUILD SUCCESS` confirms Maven build
* Unit tests pass → indicates pipeline success

---

## 🧠 **Concept Behind This Question**

* **Pipeline** automates CI/CD process in stages
* **Declarative pipeline** syntax is readable and structured
* **Checkout stage:** pulls latest code from GitHub
* **Build stage:** compiles and packages Maven project
* **Test stage:** runs unit tests
* **Post block:** executes after pipeline completion for success/failure notifications

---
Q3. (10 Marks) 
Build a Docker image for your Java project and verify its creation. 
Expected Output: Image listed in Docker images list.

Perfect! This is a **Docker build question for your Java project**. I’ll give you a **step-by-step answer** so you can perform it easily on your college PC.

---

# ✅ **Q3 (Set 3): Build Docker Image for Java Project**

**Goal:**

* Build a Docker image for your Java Maven project
* Verify the image is created using `docker images`

---

## 🧰 **Prerequisites**

1. Docker installed and running:

```bash
docker --version
```

2. Your Maven project is built (`target/MyApp-1.0-SNAPSHOT.jar`)
3. Terminal access

---

## 🪜 **Step 1: Go to Project Directory**

```bash
cd ~/Desktop/MyApp
```

---

## 🪜 **Step 2: Create a Dockerfile**

If not already created, make a file named `Dockerfile`:

```bash
cat > Dockerfile <<'EOF'
# Use official OpenJDK image
FROM openjdk:17-jdk-slim

# Set working directory
WORKDIR /app

# Copy built jar into container
COPY target/MyApp-1.0-SNAPSHOT.jar app.jar

# Run the jar
ENTRYPOINT ["java","-jar","app.jar"]
EOF
```

**Explanation:**

* Base image: `openjdk:17-jdk-slim` → lightweight Java runtime
* `WORKDIR /app` → container working directory
* `COPY target/...` → copy the compiled jar
* `ENTRYPOINT` → runs the Java program

---

## 🪜 **Step 3: Build the Docker Image**

Run this command:

```bash
docker build -t myapp-image .
```

* `-t myapp-image` → tag for the image
* `.` → current folder (where Dockerfile is located)

✅ Expected output:

```
Step 1/4 : FROM openjdk:17-jdk-slim
 ---> ...
Step 2/4 : WORKDIR /app
 ---> ...
Step 3/4 : COPY target/MyApp-1.0-SNAPSHOT.jar app.jar
 ---> ...
Step 4/4 : ENTRYPOINT ["java","-jar","app.jar"]
 ---> ...
Successfully built abcdef123456
Successfully tagged myapp-image:latest
```

---

## 🪜 **Step 4: Verify the Docker Image**

```bash
docker images
```

Expected output:

```
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
myapp-image     latest    abcdef123456   10 seconds ago   120MB
```

* `myapp-image` should be listed → image created successfully

---

## 🧠 **Concept Behind This Question**

* **Docker Image:** a snapshot of your application + runtime environment
* **`docker build`** → builds an image from Dockerfile
* **`docker images`** → lists all images on the system
* Ensures your Java project is containerized for deployment

---
Set 4 
Q1. (15 Marks) 
Create a Jenkinsfile for a pipeline that performs the following: 
• Clone code from GitHub 
• Build project using Maven 
• Build Docker image 
Expected Output: All stages execute successfully. 

Perfect! This is a **full CI/CD pipeline question** combining Git, Maven, and Docker. I’ll give you a **ready-to-use Jenkinsfile** with explanations so you can perform it on your college PC.

---

# ✅ **Q1 (Set 4): Jenkinsfile for Pipeline**

**Goal:**

* Jenkins pipeline with stages:

  1. **Clone** – pull code from GitHub
  2. **Build** – compile and package Maven project
  3. **Docker Build** – build Docker image

---

## 🧰 **Prerequisites**

1. Jenkins installed and running
2. GitHub repository exists with Maven project
3. Maven installed in Jenkins (Global Tool Configuration)
4. Docker installed on the Jenkins node and accessible to Jenkins
5. Git plugin installed

---

## 🪜 **Step 1: Create Jenkinsfile**

Use this **Declarative Pipeline** syntax:

```groovy
pipeline {
    agent any

    tools {
        maven 'Maven3' // Name of Maven installation in Jenkins
        jdk 'Java17'   // Name of JDK installation in Jenkins
    }

    environment {
        DOCKER_IMAGE = 'myapp-image:latest'
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/<username>/MyApp.git'
            }
        }

        stage('Build Maven Project') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully! All stages passed.'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
```

---

## 🪜 **Step 2: Explanation of Stages**

| Stage                   | Command                                | Purpose                                        |
| ----------------------- | -------------------------------------- | ---------------------------------------------- |
| **Clone**               | `git ...`                              | Pull latest code from GitHub                   |
| **Build Maven Project** | `mvn clean package`                    | Compile, test, and package project into `.jar` |
| **Build Docker Image**  | `docker build -t myapp-image:latest .` | Create Docker image from Dockerfile            |
| **Post**                | `echo`                                 | Notify success or failure                      |

---

## 🪜 **Step 3: Save Jenkinsfile**

* Save this file as `Jenkinsfile` at the **root of your Maven project**
* Push it to GitHub:

```bash
git add Jenkinsfile
git commit -m "Add Jenkinsfile for CI/CD pipeline"
git push origin main
```

---

## 🪜 **Step 4: Create Pipeline Job in Jenkins**

1. Jenkins → **New Item** → name: `MyApp-CICD` → **Pipeline** → OK
2. In **Pipeline section → Definition → Pipeline script from SCM**

   * SCM: Git
   * Repository URL: `https://github.com/<username>/MyApp.git`
   * Branch: `main`
   * Script Path: `Jenkinsfile`

---

## 🪜 **Step 5: Run the Pipeline**

1. Click **Build Now**
2. Go to **Console Output** → you should see:

```
[Pipeline] stage (Clone)
Cloning repository https://github.com/<username>/MyApp.git
...
[Pipeline] stage (Build Maven Project)
[INFO] BUILD SUCCESS
...
[Pipeline] stage (Build Docker Image)
Step 1/4 : FROM openjdk:17-jdk-slim
 ---> ...
Successfully built abcdef123456
Successfully tagged myapp-image:latest
[Pipeline] echo
Pipeline executed successfully! All stages passed.
```

---

## 🧠 **Concept Behind This Question**

* **Pipeline as Code:** Jenkinsfile defines the CI/CD process in code
* **Stages:** Ensure modular execution and visibility
* **Environment Variables:** `DOCKER_IMAGE` used for tagging
* **Automation:** Pull code → Build → Docker image in one pipeline

---

Q2. (15 Marks) 
Run the Docker container for the image built in the Jenkins pipeline and display application 
output using logs. 
Expected Output: Container running and producing expected output. 

Perfect! This question is about **running the Docker image built by your Jenkins pipeline** and verifying its output. I’ll give a **step-by-step guide**.

---

# ✅ **Q2 (Set 4): Run Docker Container and View Logs**

**Goal:**

* Run a container from the Docker image created in the pipeline (`myapp-image`)
* Verify it is running using `docker ps`
* Display application output using `docker logs`

---

## 🧰 **Prerequisites**

1. Docker installed and running:

```bash
docker --version
```

2. Docker image exists (`myapp-image:latest`) from the Jenkins pipeline
3. Terminal access

---

## 🪜 **Step 1: Run the Docker Container**

```bash
docker run -d --name myapp-container myapp-image:latest
```

**Explanation:**

| Option                   | Purpose                                     |
| ------------------------ | ------------------------------------------- |
| `-d`                     | Run container in detached mode (background) |
| `--name myapp-container` | Give container a friendly name              |
| `myapp-image:latest`     | Docker image built by Jenkins pipeline      |

✅ Output: Docker prints container ID, e.g.,

```
a1b2c3d4e5f6g7h8i9j0
```

---

## 🪜 **Step 2: Verify the Container is Running**

```bash
docker ps
```

Expected output:

```
CONTAINER ID   IMAGE               COMMAND                  STATUS          PORTS   NAMES
a1b2c3d4e5f6   myapp-image:latest  "java -jar app.jar"      Up 5 seconds           myapp-container
```

* **STATUS:** `Up` → container is running
* **NAMES:** `myapp-container` → matches the name you gave

---

## 🪜 **Step 3: View Container Logs**

```bash
docker logs myapp-container
```

Expected output:

```
Hello from Maven project!
```

* Confirms your Java application inside the container is running correctly

---

## 🪜 **Step 4: Optional Cleanup**

After verification:

```bash
docker stop myapp-container
docker rm myapp-container
```

* Stops and removes the container

---

## 🧠 **Concept Behind This Question**

* `docker run` → launches a container from the image created in CI/CD pipeline
* `docker ps` → lists running containers
* `docker logs` → shows stdout of the application inside the container
* Confirms **end-to-end CI/CD flow**: Jenkins builds → Docker image → container runs successfully

---

Q3. (10 Marks) 
Perform a manual Maven build from Jenkins (without pipeline) and display build success in 
console output. 
Expected Output: Maven build completed successfully in Jenkins.

Perfect! This question is about **performing a manual Maven build using a Jenkins Freestyle job** (without using a pipeline). I’ll give you a **step-by-step detailed guide**.

---

# ✅ **Q3 (Set 4): Manual Maven Build in Jenkins**

**Goal:**

* Create a Jenkins Freestyle job
* Build the Maven project manually (without pipeline)
* Verify `BUILD SUCCESS` in console output

---

## 🧰 **Prerequisites**

1. Jenkins installed and running
2. Maven installed in Jenkins (Global Tool Configuration)
3. GitHub repository exists with your Maven project
4. Git plugin installed

---

## 🪜 **Step 1: Create a Jenkins Freestyle Job**

1. Open Jenkins → click **New Item**
2. Enter name: `MyApp-Maven-Build`
3. Select **Freestyle project** → Click **OK**

---

## 🪜 **Step 2: Configure Source Code Management**

1. Scroll to **Source Code Management → Git**
2. Repository URL:

```
https://github.com/<username>/MyApp.git
```

* Use HTTPS or SSH depending on your setup
* Branch: `main`

---

## 🪜 **Step 3: Add Build Step**

1. Scroll to **Build → Add build step → Invoke top-level Maven targets**
2. Set:

   * **Goals:** `clean package`
   * **POM:** `pom.xml` (default if in project root)

> This tells Jenkins to perform a manual Maven build of your project

---

## 🪜 **Step 4: Save and Build**

1. Click **Save**
2. Click **Build Now**

---

## 🪜 **Step 5: View Console Output**

1. Click **#1 (Build Number) → Console Output**
2. You should see:

```
[INFO] Scanning for projects...
[INFO] Building MyApp 1.0-SNAPSHOT
[INFO] BUILD SUCCESS
[INFO] Total time: 3.456 s
[INFO] Finished at: 2025-10-16T08:00:00
```

* `BUILD SUCCESS` confirms the Maven build completed successfully
* No pipeline is involved; this is a **manual Freestyle build**

---

## 🧠 **Concept Behind This Question**

* Jenkins Freestyle jobs allow manual execution of build steps
* **Maven build step** compiles, tests, and packages the project
* Console output gives **real-time feedback** of build status
* Useful for **ad-hoc builds** without automating CI/CD

---
Set 5 
Q1. (15 Marks) 
Integrate a Maven project with Jenkins and schedule it to run automatically every 5 minutes 
using a build trigger. 
Expected Output: Jenkins job executes as per schedule. 

Perfect! This question is about **scheduling automatic Maven builds in Jenkins** using **Freestyle or Pipeline jobs** with build triggers. I’ll give you a **step-by-step detailed answer**.

---

# ✅ **Q1 (Set 5): Integrate Maven Project with Jenkins and Schedule Build**

**Goal:**

* Integrate a Maven project with Jenkins
* Schedule it to run automatically every **5 minutes** using build triggers

---

## 🧰 **Prerequisites**

1. Jenkins installed and running
2. Maven installed in Jenkins (Global Tool Configuration)
3. GitHub repository exists with Maven project
4. Git plugin installed

---

## 🪜 **Step 1: Create a Jenkins Job**

1. Open Jenkins → click **New Item**
2. Enter job name: `MyApp-AutoBuild`
3. Select **Freestyle project** → Click **OK**

---

## 🪜 **Step 2: Configure Source Code Management**

1. Scroll to **Source Code Management → Git**
2. Repository URL:

```
https://github.com/<username>/MyApp.git
```

* Branch: `main`

---

## 🪜 **Step 3: Add Build Step (Maven)**

1. Scroll to **Build → Add build step → Invoke top-level Maven targets**
2. Set:

   * **Goals:** `clean package`
   * **POM:** `pom.xml` (default if in root folder)

---

## 🪜 **Step 4: Configure Build Trigger**

1. Scroll to **Build Triggers**
2. Check **Build periodically**
3. Enter cron expression for every 5 minutes:

```
H/5 * * * *
```

**Explanation:**

* `H/5` → Jenkins chooses a “hashed” minute and repeats every 5 minutes to avoid all jobs starting at exactly the same time
* `* * * *` → every hour, day, month, and weekday

---

## 🪜 **Step 5: Save the Job**

* Click **Save**

---

## 🪜 **Step 6: Verify Scheduled Build**

1. Wait 5 minutes or manually click **Build Now** to test
2. Check **Build History** → new builds appear automatically every 5 minutes
3. Click **#1 (Build Number) → Console Output** → you should see:

```
[INFO] Scanning for projects...
[INFO] Building MyApp 1.0-SNAPSHOT
[INFO] BUILD SUCCESS
[INFO] Total time: 3.456 s
```

---

## 🧠 **Concept Behind This Question**

* Jenkins **automates Maven builds** by scheduling jobs
* **Build Triggers → Build periodically** uses a cron-like syntax
* Ensures **continuous integration** even without manual intervention

---
Q2. (15 Marks) 
Write a Dockerfile, build the image, and push it to Docker Hub from the command line. 
Expected Output: Image successfully pushed to Docker Hub.

Perfect! This question is about **Dockerizing your Java project and pushing it to Docker Hub**. I’ll give you a **detailed step-by-step guide** for the command line.

---

# ✅ **Task: Dockerfile → Build → Push to Docker Hub**

**Goal:**

* Create a Dockerfile for your Java project
* Build a Docker image
* Push it to Docker Hub
* Verify the image is on Docker Hub

---

## 🧰 **Prerequisites**

1. Docker installed and running:

```bash
docker --version
```

2. Docker Hub account created
3. Your Maven project is built (`target/MyApp-1.0-SNAPSHOT.jar`)
4. GitHub repo optional (if used in pipeline)

---

## 🪜 **Step 1: Create a Dockerfile**

Go to your project folder:

```bash
cd ~/Desktop/MyApp
```

Create `Dockerfile`:

```bash
cat > Dockerfile <<'EOF'
# Use official OpenJDK image
FROM openjdk:17-jdk-slim

# Set working directory
WORKDIR /app

# Copy jar into container
COPY target/MyApp-1.0-SNAPSHOT.jar app.jar

# Run the jar
ENTRYPOINT ["java","-jar","app.jar"]
EOF
```

---

## 🪜 **Step 2: Build Docker Image**

Tag your image with Docker Hub username:

```bash
docker build -t <dockerhub-username>/myapp:latest .
```

Example:

```bash
docker build -t sakshi123/myapp:latest .
```

✅ Expected output:

```
Successfully built abcdef123456
Successfully tagged sakshi123/myapp:latest
```

---

## 🪜 **Step 3: Login to Docker Hub**

Login to Docker Hub from terminal:

```bash
docker login
```

* Enter **username** and **password**
* For accounts with 2FA, use a **personal access token** as password
* Success message:

```
Login Succeeded
```

---

## 🪜 **Step 4: Push Docker Image to Docker Hub**

Push the image:

```bash
docker push <dockerhub-username>/myapp:latest
```

Example:

```bash
docker push sakshi123/myapp:latest
```

✅ Expected output:

```
The push refers to repository [docker.io/sakshi123/myapp]
...
latest: digest: sha256:abcdef123456 size: 1234
```

* Confirms image is uploaded to Docker Hub

---

## 🪜 **Step 5: Verify Image on Docker Hub**

1. Go to [https://hub.docker.com](https://hub.docker.com)
2. Log in and go to **Repositories** → `myapp`
3. You should see `latest` tag with image pushed

---

## 🧠 **Concept Behind This Question**

* Dockerfile → defines image structure (Java runtime + your jar)
* `docker build` → builds image locally
* `docker tag` → optional, for naming images with Docker Hub repo
* `docker push` → uploads image to Docker Hub
* Makes the app **portable and shareable**

---

Q3. (10 Marks) 
Create a simple Git repository, commit and push at least two versions of the same file showing 
version control in GitHub. 
Expected Output: Multiple commits visible in GitHub history. 

Perfect! This question is about **demonstrating version control using Git**. I’ll give you a **step-by-step guide** to create a repository, make changes, and push multiple commits to GitHub.

---

# ✅ **Q3 (Set 5): Git Version Control with Multiple Commits**

**Goal:**

* Create a Git repository
* Commit and push at least **two versions of the same file**
* Show multiple commits in GitHub

---

## 🧰 **Prerequisites**

1. Git installed on your PC:

```bash
git --version
```

2. GitHub account created
3. Terminal / Git Bash / PowerShell access

---

## 🪜 **Step 1: Create a Local Git Repository**

Create a new folder for the project:

```bash
mkdir MyGitProject
cd MyGitProject
```

Initialize Git:

```bash
git init
```

✅ Output:

```
Initialized empty Git repository in /path/MyGitProject/.git/
```

---

## 🪜 **Step 2: Create a File (Version 1)**

Create a simple file, e.g., `hello.txt`:

```bash
echo "Hello World - Version 1" > hello.txt
```

Check status:

```bash
git status
```

Stage the file:

```bash
git add hello.txt
```

Commit the first version:

```bash
git commit -m "Add hello.txt version 1"
```

✅ Output:

```
[main (root-commit) abc1234] Add hello.txt version 1
 1 file changed, 1 insertion(+)
 create mode 100644 hello.txt
```

---

## 🪜 **Step 3: Modify the File (Version 2)**

Edit the file:

```bash
echo "Hello World - Version 2" >> hello.txt
```

Check status:

```bash
git status
```

Stage the change:

```bash
git add hello.txt
```

Commit the second version:

```bash
git commit -m "Update hello.txt to version 2"
```

✅ Output:

```
[main def5678] Update hello.txt to version 2
 1 file changed, 1 insertion(+)
```

---

## 🪜 **Step 4: Create GitHub Repository**

1. Go to [https://github.com](https://github.com) → **New Repository**
2. Name it `MyGitProject`
3. Keep it **public** or **private**
4. **Do not initialize with README**

---

## 🪜 **Step 5: Link Local Repo to GitHub**

```bash
git remote add origin https://github.com/<username>/MyGitProject.git
```

Push commits:

```bash
git branch -M main
git push -u origin main
```

✅ Output:

```
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/<username>/MyGitProject.git
   abc1234..def5678  main -> main
```

---

## 🪜 **Step 6: Verify on GitHub**

1. Go to your GitHub repository:
   `https://github.com/<username>/MyGitProject`
2. Click **Commits** → you should see:

   * `Add hello.txt version 1`
   * `Update hello.txt to version 2`

✅ This shows **multiple versions of the same file**

---

## 🧠 **Concept Behind This Question**

* **Git version control** tracks changes to files over time
* Each commit represents a **snapshot of the project**
* Multiple commits show **progression/version history**
* `git push` syncs your local repository with **remote GitHub repository**

---








