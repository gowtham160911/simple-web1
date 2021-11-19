
pipeline{
    tools{
        jdk 'myjava'
        maven 'mymaven'
    }
    agent {label 'Unix'}
      stages{
           stage('Checkout'){
	    agent any
               steps{
		 echo 'cloning..'
                 git 'https://github.com/SreejaKolli/Sample_maven_project.git'
              }
          }
          stage('Compile'){
              agent any
              steps{
                  echo 'compiling..'
                  sh 'mvn compile'
	      }
          }
          stage('CodeReview'){
              agent any
              steps{
		  echo 'codeReview'
                  sh 'mvn pmd:pmd'
              }
          }
           stage('UnitTest'){
		   agent any
              steps{
	         
                  sh 'mvn test'
              }
               post {
               success {
                   junit 'target/surefire-reports/*.xml'
               }
           }	
          }
           stage('MetricCheck'){
               agent any
        //For Ubuntu with Java Version 11,code coverage will not execute cobertura plugin as it has
        //exception to execute only with java version 8
            steps{
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    sh 'mvn cobertura:cobertura -Dcobertura.report.format=xml'
                 }
               }
               post {
               success {
	           cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false                  
                 }
               }		
          }
          stage('Package'){
              agent any
              steps{
                  sh 'mvn package'
              }
          }
	     
          
      }
}
