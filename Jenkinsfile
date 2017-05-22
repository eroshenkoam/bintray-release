pipeline {
    agent { label 'java' }
    tools { maven 'default' }
    parameters {
        booleanParam(name: 'RELEASE', defaultValue: false, description: 'Perform release?')
        string(name: 'RELEASE_VERSION', defaultValue: '', description: 'Release version')
        string(name: 'DEVELOPMENT_VERSION', defaultValue: '', description: 'Development version (without SNAPSHOT)')
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true clean verify'
            }
        }
        stage('Test') {
            steps {
                sshagent(['qameta-ci_ssh']) {
                    sh 'ssh -T git@github.com'
                }
            }
        }
        stage('Release') {
            when { expression { return params.RELEASE } }
            steps {
                configFileProvider([configFile(fileId: 'bintray-settings.xml', variable: 'SETTINGS', replaceTokens: true)]) {
                    withCredentials([usernamePassword(credentialsId: 'qameta-ci_github', passwordVariable: 'GITHUB_PASSWORD', usernameVariable: 'GITHUB_USERNAME')]) {
                        sh "mvn -B release:prepare " +
                                "-s ${env.SETTINGS} " +
                                "-Dtag=${params.RELEASE_VERSION} " +
                                "-Dusername=${env.GITHUB_USERNAME} " +
                                "-Dpassword=${env.GITHUB_PASSWORD}"
                    }
                }

            }
        }
    }
    post {
        always {
            deleteDir()
        }
    }
}
