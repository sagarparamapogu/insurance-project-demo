node{
    
    stage('checkout'){
        git 'https://github.com/sagarparamapogu/insurance-project-demo.git'
    }

    stage('maven build'){
        sh 'mvn clean package'
    }

    stage('containerize'){
        sh 'docker build -t sagarparamapogu/insure-me:1.0 .'
    }

    stage('Release'){
       withCredentials([string(credentialsId: 'dockerhubPwd', variable: 'dockerhubPwd')]) {
        sh "docker login -u sagarparamapogu -p ${dockerhubPwd}"
        sh 'docker push sagarparamapogu/insure-me:1.0'
        }
    }

    stage('Deploy to Test'){
        ansiblePlaybook become: true, credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-test-server.yml', vaultTmpPath: ''
    }

    stage('checkout regression test source code'){
        git 'https://github.com/sagarparamapogu/my-selenium-test-app.git'
    }

    stage('build test scripts'){
        sh 'mvn clean package assembly:single'
    }

    stage('execute selenium test script'){
        sh 'java -jar target/my-app-test-0.0.1-SNAPSHOT-jar-with-dependencies.jar'
    }

    stage('checkout'){
        git 'https://github.com/sagarparamapogu/insurance-project-demo.git'
    }

     stage('Deploy to Prod'){
     ansiblePlaybook become: true, credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-prod-server.yml', vaultTmpPath: ''
    }


}
