node{
    stage('scm checkout'){
        git credentialsId: 'gitcreds', url: 'https://github.com/prem0132/dotnet-demo.git'
    }
    stage('install packages'){
        sh 'npm install'
        sh 'npm run build'
    }
    stage('docker build'){
        sh 'docker build -t premhashmap/reactapp:latest .'
    }
    stage('docker push'){
        withCredentials([string(credentialsId: 'dockerhubPWD', variable: 'dockerpwd')]) {
            sh 'docker login -u premhashmap -p ${dockerpwd}'
        }
        sh 'docker push premhashmap/reactapp:latest'
    }
    stage('Rolling Update'){
        kubernetesDeploy configs: '', kubeConfig: [path: ''], kubeconfigId: 'kubeconfig', secretName: '', ssh: [sshCredentialsId: '*', sshServer: ''], textCredentials: [certificateAuthorityData: '', clientCertificateData: '', clientKeyData: '', serverUrl: 'https://']
        sh 'kubectl get pods -oname |grep react |xargs kubectl delete'
    }
}

