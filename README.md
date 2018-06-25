Brings up three Vagrant VMs with one Jenkins master and two slaves. All are configured to run Docker pipeline jobs.

## Prerequisites

* Vagrant >= 2.x
* Ansible >= 2.5.x

## Usage

Bring up and provision the VMs using:

```bash
vagrant up
```

The Jenkins UI can be accessed at http://localhost:8080 with the username `admin` and password `admin`.

You should be able to create a new pipeline job with an inline Jenkinsfile that contains:

```groovy
pipeline {
  agent {
    docker { image 'ubuntu' }
  }
  stages {
    stage('main') {
      steps {
        sh 'uname -a'
        sh 'cat /etc/issue'
      }
    }
  }
}
```