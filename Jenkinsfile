pipeline {
    agent any
    parameters {
        gitParameter(branch: '', 
        branchFilter: '.*', 
        defaultValue: 'master', 
        description: 'enter the branch name', 
        name: 'branch', quickFilterEnabled: false, 
        selectedValue: 'NONE', 
        sortMode: 'NONE', 
        tagFilter: '*', 
        type: 'PT_BRANCH')
        
        booleanParam(defaultValue: true, description: 'Please select if you want to Execute Sonar:', name: 'Execute_Sonar')
    }
    stages {
        stage('checkout') {
            steps {
                checkout([$class: 'GitSCM', 
                branches: [[name: "${params.branch}"]],
                extensions: [], 
                userRemoteConfigs: [[credentialsId: 'Git_Creds', 
                url: 'https://github.com/AnchalKundra/cobertura-example.git']]])
            }
		}
		stage('Cobertura') {
		    steps {
		        tool name: 'Java_home', type: 'jdk'
		        tool name: 'Maven_home', type: 'maven'
		        sh 'mvn cobertura:cobertura -Dcobertura.report.formats=xml'
		    }
		}
		stage('Publish Cobertura') {
		    steps {
		        cobertura autoUpdateHealth: false, 
		        autoUpdateStability: false, 
		        classCoverageTargets: '80, 0, 0', 
		        coberturaReportFile: '**/target/site/cobertura/coverage.xml', 
		        conditionalCoverageTargets: '70, 0, 0', 
		        fileCoverageTargets: '80, 0, 0', 
		        lineCoverageTargets: '80, 0, 0', 
		        maxNumberOfBuilds: 0, 
		        methodCoverageTargets: '80, 70, 40', 
		        onlyStable: false, 
		        packageCoverageTargets: '80, 0, 0', 
		        sourceEncoding: 'ASCII', 
		        zoomCoverageChart: false
		    }
		}
		stage('Sonar') {
		    when {
                expression { params.Execute_Sonar == true}
            }
		    steps {
		        echo "Teting Sonar"
		    }
		}
		stage('Build') {
		    steps {
		        echo "build code"
		    }
		}
		stage('Publish Nexus') {
		    steps {
		        echo "Publishing to Artifactry"
		    }
		}
		stage('Docker Image') {
		    steps {
		        echo "Creating Docker Image"
		    }
		}
		stage('Deploy to Kubernetus') {
		    steps {
		        echo "Deploying on Pods"
		    }
		}
    }
}
