node{
    stage('scm checkout'){
        git credentialsId: 'gitcreds', url: 'https://github.com/prem0132/dotnet-demo.git'
    }
    stage('install packages'){
        sh 'npm install'
        sh 'npm run build'
    }
}
