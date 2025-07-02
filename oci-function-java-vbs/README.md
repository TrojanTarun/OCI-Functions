---

# 🚀 Java-based OCI Function Deployment using Oracle Visual Builder Studio (VBS)

This repository demonstrates how to develop, deploy, and manage a Java-based Oracle Cloud Infrastructure (OCI) Function using Oracle Visual Builder Studio (VBS) and Git. It includes the complete directory structure, build and deploy automation via CI/CD pipelines, and OCI best practices.

---

## 🧰 Technologies Used

- Java 11
- Oracle Cloud Functions (Fn Project)
- Oracle Visual Builder Studio (VBS)
- Git (hosted in VBS)
- Maven (Fn Maven Plugin)
- OCI CLI & Fn CLI (for local testing, optional)

---

## 📁 Project Structure
``````
oci-function-java-vbs/
├── function/
│   ├── func.yaml                  # Function metadata (name, runtime, etc.)
│   ├── pom.xml                    # Maven configuration
│   └── src/
│       └── main/
│           └── java/
│               └── com/
│                   └── example/
│                       └── Function.java    # Java function handler
├── .vbs/
│   └── buildspec.yaml             # VBS build pipeline config
├── oci-config/
│   └── config.properties          # Optional local config for OCIDs
└── README.md
``````

---

## 🚦 Pre-requisites

| Tool | Required For |
|------|--------------|
| Oracle Cloud Account | Hosting and deploying OCI Function |
| Oracle Visual Builder Studio | Git repository and CI/CD pipeline |
| OCI CLI | (Optional) Local testing and deployment |
| Fn CLI | (Optional) Local build and invoke |
| Java 11 & Maven 3.6+ | Building Java function |

---

## 🧑‍💻 Step 1: Create Project in Oracle VBS

1. Log in to Oracle Visual Builder Studio.
2. Go to Projects > Create Project > Git-Hosted Project.
3. Name the project oci-java-function-project.
4. Initialize a Git repository inside the project.

---

## 🔄 Step 2: Clone Repository Locally
bash
``````
git clone https://<your-vbs-project-url>.git
cd oci-java-function-project
``````

---

🏗️ Step 3: Add Java Function Code

func.yaml
``````
name: my-java-function
version: 0.0.1
runtime: java
memory: 256
timeout: 30
``````

---

Function.java
``````
package com.example;

public class Function {
    public String handleRequest(String input) {
        return "Hello, " + input + " from OCI Java Function!";
    }
}
``````

---

pom.xml
``````
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example</groupId>
  <artifactId>oci-function</artifactId>
  <version>1.0</version>
  <packaging>jar</packaging>

  <dependencies>
    <dependency>
      <groupId>com.fnproject.fn</groupId>
      <artifactId>api</artifactId>
      <version>1.0.121</version>
    </dependency>
  </dependencies>

  <build>
    <plugins>
      <plugin>
        <groupId>com.fnproject.fn</groupId>
        <artifactId>fn-maven-plugin</artifactId>
        <version>1.0.121</version>
      </plugin>
    </plugins>
  </build>
</project>
``````

---

⚙️ Step 4: Set Up .vbs/buildspec.yaml
``````
version: 0.1
steps:
  - name: Build Java Function
    image: maven:3.8.1-openjdk-11
    shell: bash
    workingDir: function
    commands:
      - mvn clean package
      - fn build

  - name: Deploy to OCI Function
    image: oraclelinux:7
    shell: bash
    workingDir: function
    env:
      - name: OCI_REGION
        value: ap-hyderabad-1
      - name: OCI_APP
        value: my-oci-function-app
    commands:
      - fn deploy --app $OCI_APP --region $OCI_REGION
``````

---

🔁 Step 5: Git Push to Trigger Pipeline
``````
git add .
git commit -m "Initial OCI Function with VBS"
git push origin main
``````
If Git triggers are enabled in VBS, this will automatically run the build and deploy pipeline.


---

🏗️ Step 6: Configure VBS Build Pipeline

1. In your project, go to Build > Pipelines > Create Pipeline.

2. Name it: oci-function-pipeline.

3. Select the repo and main branch.

4. Point to .vbs/buildspec.yaml.

5. Add OCI_REGION and OCI_APP as pipeline environment variables.

6. Enable Git trigger for automatic CI/CD.




---

🧪 Step 7: Test the Function

From OCI Console:

1. Go to Developer Services > Functions.

2. Select your application and function.

3. Use Test tab to provide payload like "World".



Expected output:

Hello, World from OCI Java Function!


---

🛠 Optional: Local Test with Fn CLI

fn start
cd function
mvn clean package
fn build
fn deploy --app my-oci-function-app --region ap-hyderabad-1
fn invoke my-oci-function-app my-java-function


---

🔐 Security & Configuration

Store sensitive values like OCIDs and secrets as VBS Environment Variables.

For production, use OCI Vault to manage secrets.

Avoid committing secrets in config.properties or source code.



---




