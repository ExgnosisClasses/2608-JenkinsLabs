# Lab 4 - The Jenkins API

This lab introduces you to the functionality of the Jenkins API.

You will be calling the API from the command line using `curl`, but this is easily done from almost any programming language in use today - Java, GO, Python, JavaScript, etc.

## Step 1: Set up the Job

In the Jenkins UI, create a basic pipeline job using the supplied script in the script window.

```bash
pipeline {
  agent any

  // Use an environment variable instead of job parameters.
  // Jenkins will take this from the node env, job env injection, or a default here.
  environment {
    GREETING_NAME = "World"
  }

  stages {
    stage('Hello') {
      steps {
        echo "Hello, ${env.GREETING_NAME}!"

        // Create an artifact using Windows commands
        bat '''
          if not exist build mkdir build
          powershell -NoProfile -Command "Get-Date | Out-File -Encoding utf8 build\\build.txt"
        '''
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'build/**', fingerprint: true
    }
  }
}
```

Run the job and confirm that it build successfully and inspect the console output, the pipline stages screen and the build artifacts.

## Part 2: GET commands

For these, you can just enter the URL in a browser or use the `curl` command.

Note that if you are using the `curl` command in Powershell, you have to use `curl.exe` to avoid one of the Powershell aliases. 

In order to execute some of the `curl` commands, you need to provide a token. This is defined in your use profile, but you can create a new one if needed. These labs have been tested with the one that is currently defined.

The token is available to cut and paste from in the `APITokes.txt` file in the Jenkins directory and is listed here

```bash
Student:11fab93c57c13c80c54ae4836fd1593b2a
```


### Jenkins Root Server API

`localhost:8080/api/json`

<img src="images/api1.png" />

<img src="images/api2.png" />

### List jobs (use tree to keep output small)

Try the following commands and examine the output. 


