pipeline {
  agent {
    node {
      label 'aip-ta'
    }
  }
  
  environment {
    BUILD = "release"
  }

  stages {
    stage('Hi') {
      steps {
        echo "AGENT: ${env.NODE_NAME}"
      }
    }

    stage('Git pull') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: 'master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'CleanBeforeCheckout'], [$class: 'SubmoduleOption', disableSubmodules: false, parentCredentials: true, recursiveSubmodules: true, reference: '', trackingSubmodules: false]], submoduleCfg: [], userRemoteConfigs: [[url: 'git@github.com:mwhistler/aip-ta.git']]])
      }
    }

    stage('BASIC') {
      steps {
        catchError(stageResult: 'FAILURE') {
          sh script: 'python -m pytest tests/chargesafe/test_chargesafe_happy_path.py', label: "test_chargesafe_happy_path"
        }
      }
    }
    
  }

  post {
    always {
      archiveArtifacts 'results.xml'
      junit(testResults: 'results.xml', allowEmptyResults: true, keepLongStdio: true)
    }
  }
}
