pipeline {
    agent { label 'java' }
    tools { maven 'default' }
//    parameters {
//        booleanParam(name: 'RELEASE', defaultValue: 'false', description: '')
//        string(name: 'RELEASE_VERSION', defaultValue: '', description: 'Release version')
//        string(name: 'DEVELOPMENT_VERSION', defaultValue: '', description: 'Development version (without SNAPSHOT)')
//    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true clean verify'
            }
        }
//        stage('Release') {
//            when { expression { return params.RELEASE } }
//            steps {
//                configFileProvider([configFile(fileId: 'bintray-settings.xml', variable: 'SETTINGS', replaceTokens: true)]) {
//                    sh 'mvn release:prepare release:perform ' +
//                            '-s ${SETTINGS} ' +
//                            '-DreleaseVersion=${params.RELEASE_VERSION} ' +
//                            '-DdevelopmentVersion=${params.DEVELOPMENT_VERSION-SNAPSHOT}'
//                }
//            }
//        }
    }
}
