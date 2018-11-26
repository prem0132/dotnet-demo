pipeline{
        agent {
          docker {
            image 'library/node'
            args '-u root -v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
       stages{     
            stage('scm checkout'){
                git credentialsId: 'gitcreds', url: 'https://github.com/prem0132/dotnet-demo.git'
            }
            stage('install packages'){
                steps{
                sh 'npm install'
                sh 'npm run build'
                }
            }
            stage('docker build'){
                sh 'docker build -t premhashmap/reactapp:latest .'
            }
            stage('docker push'){
                steps{
                withCredentials([string(credentialsId: 'dockerhubPWD', variable: 'dockerpwd')]) {
                    sh 'docker login -u premhashmap -p ${dockerpwd}'
                }
                sh 'docker push premhashmap/reactapp:latest'
                }
            }
            stage('Rolling Update'){
            sh 'kubectl --kubeconfig /var/lib/jenkins/jobs/config get pods -oname |grep react |xargs kubectl --kubeconfig /var/lib/jenkins/jobs/config delete'
            }
       }
    }