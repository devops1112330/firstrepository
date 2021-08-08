#!usr/bin/env reoovy

node{
    stage('git checkout'){
        git 'https://github.com/devops1112330/DevOpsClassCodes.git'
    }
    stage('compile'){
       withMaven(maven:'mymevan'){
         sh 'mvn compile'
       }
    }
     stage ('review'){
        try{
            withMaven(maven:'mymevan'){
                sh 'mvn pmd:pmd'
            }
        }finally{
            recordIssues(tools: [acuCobol(pattern: 'target/pmd.xml')])
        }
    }
    
    stage ('test'){
        try{
            withMaven(maven:'mymevan'){
                sh 'mvn test'
            }
        }finally{
            junit 'target/surefire-reports/*.xml'
        }
    }
    stage ('coveragecheck'){
        try{
            withMaven(maven:'mymevan'){
                sh 'mvn cobertura:cobertura -Dcobertura.report.format=xml'
            }
        }finally{
            cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false
        }
    }
    stage ('package'){
        withMaven(maven:'mymevan'){
             sh 'mvn package'
        }
    }
}

