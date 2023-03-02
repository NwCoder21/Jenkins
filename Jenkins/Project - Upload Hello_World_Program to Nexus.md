# Jenkisfile Script:

```java
pipeline {
    agent any 
    tools {
        gradle 'Gradle 6.6.1'
    }     stages {
        stage('Pull Code from Git') {
            steps {
                git credentialsId: 'Bitbucket_Creds', url: 'http://rsdv0rgops03:8080/scm/usman/test3.git'
            }
        }
        stage('Compile Code') {
            steps {
                   sh 'gradle -Dorg.gradle.java.home=/u01/java/jdk-8 build ' 
            }
        }
        stage('Archiving and Zipping Files') {
            steps {
                   archiveArtifacts artifacts: 'build/**/*.jar'
                   sh "zip -r test3-${BUILD_NUMBER}.zip build"
                   sh "chmod 777 test3-${BUILD_NUMBER}.zip"
                   sh "ls -l"
            }
        }
        stage('Upload Artefacts to Nexus') {
            steps {
                   nexusPublisher nexusInstanceId: 'NEXUS', nexusRepositoryId: 'test-three', 
                   packages: [
                [
                  $class: "MavenPackage",
                  mavenAssetList: [
                    [
                        classifier: "",
                        extension: "zip",
                        filePath: "test3-${BUILD_NUMBER}.zip"
                    ]                    
                  ],
                  mavenCoordinate: [
                    groupId: "com.hello",
                    artifactId: "helloWorldProgram",
                    version: "${BUILD_NUMBER}",
                    packaging: "zip"
                  ]
                ]
              ]             
             }
        }
    }
}

```

