# CodeAlpha Task 2: Jenkins Remoting Project

## 📋 Overview

This project demonstrates the implementation of a **Jenkins Master-Agent (Remoting) Architecture** using Docker. The objective was to distribute build workloads securely across different machines and verify that jobs triggered on the Jenkins Master can successfully execute on a Remote Agent Node.

The project showcases core DevOps concepts such as distributed builds, Jenkins Remoting, Docker networking, CI/CD automation, and troubleshooting real-world infrastructure issues.

---

## 🏗️ Architecture

The setup consists of two separate Docker containers communicating over a dedicated Docker bridge network.

### Jenkins Master (Controller)

* Provides the Jenkins Web UI.
* Manages jobs, pipelines, and build scheduling.
* Controls communication with remote agents.

### Jenkins Remote Agent (Node)

* Receives build tasks from the Jenkins Master.
* Executes the assigned workloads.
* Returns logs and execution results back to the Master.

Communication between the Master and Agent is established through a secure Jenkins Remoting connection.

---

## 🛠️ Tech Stack

* **CI/CD Tool:** Jenkins LTS
* **Containerization:** Docker
* **Orchestration:** Docker Compose
* **Agent Operating System:** Linux (Docker Container)
* **Programming Language:** Groovy (Jenkins Pipeline)
* **Java Runtime:** JDK 21

---

## 🚀 How It Works

1. Docker Compose creates both Jenkins Master and Jenkins Agent containers.
2. Both containers are connected through a shared Docker network.
3. The Agent authenticates with the Master using a secure Jenkins secret.
4. A Jenkins Pipeline job is configured to run on the Remote Agent using:

```groovy
agent { label 'remote-linux-node' }
```

5. The Master schedules the job and transfers execution to the Agent.
6. The Agent executes the commands and returns logs to the Master.
7. Jenkins displays the execution results in the build console.

---

## 🔧 Jenkins Pipeline Used

```groovy
pipeline {
    agent { label 'remote-linux-node' }

    stages {
        stage('Verify Remote Node') {
            steps {
                sh 'echo "Yeh command REMOTE AGENT par run ho rahi hai!"'
                sh 'hostname'
                sh 'whoami'
            }
        }
    }
}
```

---

## 🕵️‍♂️ DevOps Troubleshooting Highlight

During the initial setup, the Jenkins Agent failed to connect with the Jenkins Master.

### Error Encountered

```text
UnsupportedClassVersionError
```

### Root Cause

The Jenkins Master was running on **Java 21 (class version 65.0)** while the Jenkins Agent image was using **Java 11 (class version 55.0)**.

Because both components were using different Java versions, Jenkins Remoting could not establish communication.

### Solution

The Agent Docker image was upgraded from:

```text
jenkins/inbound-agent:latest-jdk11
```

to:

```text
jenkins/inbound-agent:latest-jdk21
```

After upgrading the Agent to Java 21, the connection was established successfully and the Agent came online.

This troubleshooting exercise provided practical experience in debugging real-world DevOps compatibility issues.

---

## 📸 Output Verification

### Node Verification

* Jenkins Master and Agent successfully connected.
* Agent status displayed as **Online / In Sync**.

### Pipeline Verification

The following commands were executed on the Remote Agent:

```bash
hostname
whoami
```

### Result

* Build Status: **SUCCESS**
* Execution Node: **remote-linux-node**
* Jenkins successfully delegated the workload to the Remote Agent.

---

## 🎯 Skills Learned

* Jenkins Master-Agent Architecture
* Jenkins Remoting
* Docker Networking
* Docker Compose
* CI/CD Fundamentals
* Pipeline as Code
* Distributed Build Execution
* Java Runtime Compatibility Troubleshooting
* Container-Based Infrastructure Management

---

## ▶️ How to Run

### Prerequisites

* Docker Desktop Installed
* Docker Compose Installed

### Steps

1. Clone the repository.

```bash
git clone https://github.com/KhushbooShah123/CodeAlpha_Jenkins_Remoting.git
```

2. Navigate to the project directory.

```bash
cd <project-folder>
```

3. Start Jenkins Master and Agent containers.

```bash
docker-compose up -d
```

4. Open Jenkins in the browser.

```text
http://localhost:8080
```

5. Complete Jenkins initial setup.

6. Generate the Agent Secret from Jenkins.

7. Update the Agent secret inside `docker-compose.yml`.

8. Restart the Agent container.

```bash
docker-compose up -d jenkins-agent
```

9. Create and run the pipeline job.

10. Verify successful execution from Jenkins Console Output.

---

## 👤 Intern

**Khushboo Shah**
CodeAlpha DevOps Intern
