pipeline {
    agent { label 'java' }
    tools { maven 'default' }
    parameters {
        booleanParam(name: 'RELEASE', defaultValue: false, description: 'Perform release?')
        string(name: 'RELEASE_VERSION', defaultValue: '', description: 'Release version')
        string(name: 'DEVELOPMENT_VERSION', defaultValue: '', description: 'Development version (without SNAPSHOT)')
    }
    stages {
        stage('Release') {
            when { expression { return params.RELEASE } }
            steps {
                sh 'git checkout master && git pull origin master'
                configFileProvider([configFile(fileId: 'bintray-settings.xml', variable: 'SETTINGS', replaceTokens: true)]) {
                    sshagent(['qameta-ci_ssh']) {
                        sh "mvn release:prepare -B -s ${env.SETTINGS} " +
                                "-DreleaseVersion=${params.RELEASE_VERSION} " +
                                "-DdevelopmentVersion=${params.DEVELOPMENT_VERSION}-SNAPSHOT"
                    }
                    sh "mvn release:perform -B -s ${env.SETTINGS}"
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
