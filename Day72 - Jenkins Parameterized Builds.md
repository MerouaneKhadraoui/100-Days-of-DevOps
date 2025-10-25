# Day 72: Jenkins Parameterized Builds

## Question

A new DevOps Engineer has joined the team and he will be assigned some Jenkins related tasks. Before that, the team wanted to test a simple parameterized job to understand basic functionality of parameterized builds. He is given a simple parameterized job to build in Jenkins. Please find more details below:

**Task:**

1. Create a `parameterized` job which should be named as `parameterized-job`

2. Add a `string` parameter named `Stage`; its default value should be `Build`.

3. Add a `choice parameter` named `env`; its choices should be `Development`, `Staging` and `Production`.

4. Configure job to execute a shell command, which should echo both parameter values (you are passing in the job).

5. Build the Jenkins job at least once with choice parameter value `Staging` to make sure it passes.

`Note:` 

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in the back end. In this case please make sure to refresh the UI page.

2. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

---

## ✅ Task Requirements

1. Create a **parameterized** job named `parameterized-job`
2. Add a **string** parameter named `Stage` with default value `Build`
3. Add a **choice** parameter named `env` with choices: `Development`, `Staging`, `Production`
4. Configure job to execute a shell command that echoes both parameter values
5. Build the job at least once with the choice parameter value `Staging`

---

## 🚀 Freestyle Job Solution

### **Step 1: Create the Jenkins Job**
- Go to Jenkins Dashboard → **New Item**
- Name: `parameterized-job`
- Type: **Freestyle project**
- Click **OK**

### **Step 2: Enable Parameters**
- In **General** section, check:  
  `This project is parameterized`

- Add parameters:

#### String Parameter
| Field | Value |
|--------|--------|
| Name | Stage |
| Default Value | Build |

#### Choice Parameter
| Field | Value |
|--------|--------|
| Name | env |
| Choices | Development <br> Staging <br> Production |

---

### **Step 3: Add Shell Command**
Under **Build → Add build step → Execute shell**:

```bash
echo "Stage: $Stage"
echo "Environment: $env"
```

---

### **Step 4: Save and Run the Job**
1. Click **Save**
2. Click **Build with Parameters**
3. Choose `env = Staging`
4. Build the job

#### Expected Output (Console Log)
```
Stage: Build
Environment: Staging
Finished: SUCCESS
```

---

## 💡 Jenkins Pipeline (Jenkinsfile) Equivalent

If you prefer a declarative pipeline instead of a Freestyle job:

```groovy
pipeline {
    agent any
    parameters {
        string(name: 'Stage', defaultValue: 'Build', description: 'Stage name')
        choice(name: 'env', choices: ['Development', 'Staging', 'Production'], description: 'Select environment')
    }
    stages {
        stage('Echo Parameters') {
            steps {
                sh '''
                    echo "Stage: ${Stage}"
                    echo "Environment: ${env}"
                '''
            }
        }
    }
}
```

### ✅ Example Output
```
Stage: Build
Environment: Staging
Finished: SUCCESS
```

---

## 🎯 Result
You have successfully created and executed a **parameterized Jenkins job** that demonstrates:
- Use of **String** and **Choice** parameters
- Execution of a **shell build step**
- Proper display of parameter values in the **build output**