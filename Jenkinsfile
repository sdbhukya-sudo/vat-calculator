pipeline {
    agent any

    stages {
        // stage('Checkout'){
        //     steps {
        //       git url: 'https://github.com/sdbhukya-sudo/vat-calculator.git',
        //           branch: 'main'
        //     }
         // }
        
        stage('Run Tests') {
            steps {
              sh 'npm install'
              sh 'CI=true npm test'
           }
        }
        stage('Build Webpack') {
            steps {
                sh 'npm run build'
            }
        }
        stage('Archive') {
            steps {
              sh 'tar -czf build.tar.gz build'
              archiveArtifacts 'build.tar.gz'
            }
        }
    }
    post {
    always {
        recordIssues(
            qualityGates: [
                [criticality: 'FAILURE', integerThreshold: 30, threshold: 10.0, type: 'TOTAL_HIGH'], 
                [criticality: 'FAILURE', integerThreshold: 5, threshold: 5.0, type: 'NEW']
                ], 
                sourceCodeRetention: 'LAST_BUILD', 
                tools: [grype()]
        )
    }
}

}
