pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh '''# Create python virtual Environment
pip3 install virtualenv --user
python3 -m venv repo
. repo/bin/activate

# Install python requirements
pip install -r requirements.txt'''
      }
    }
    stage('Test') {
      steps {
        sh '''# Activate python virtual environment
. repo/bin/activate

# Run pytest and save coverage report
pytest --cov-report xml --cov-report term --cov ./lib/'''
      }
    }
    stage('Publish test report') {
      steps {
        cobertura(failUnstable: true, failUnhealthy: true, failNoReports: true, lineCoverageTargets: '90,50,80', coberturaReportFile: 'coverage.xml')
      }
    }
    stage('Deploy') {
      steps {
        input(message: 'Please', id: '1')
        sh '''cd deployment
sh provision.sh'''
      }
    }
  }
}