```jenkins
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
                    version: "Build Number: ${BUILD_NUMBER}",
                    packaging: "zip"
                  ]
                ]
              ]             }
        }
        stage('Download Artefacts from Nexus') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus_Credentials', passwordVariable: 'pass_word', usernameVariable: 'user_name')]) {
                sh "wget --user=user_name --password=pass_word http://rsdv0rgops02:8080/repository/test-three/com/hello/helloWorldProgram/Build%20Number:%20${BUILD_NUMBER}/helloWorldProgram-Build%20Number:%20${BUILD_NUMBER}.zip"
            }
        }
    }         stage('Upload Artefacts to Liferay') {
            steps {
            sh 'scp /u01/jenkins/workspace/test3_Hello_World@2/${BUILD_NUMBER}.zip usman.farooq@rsdv0wgws13:c/Users/usman.farooq/Desktop/Life_Ray/liferay-dxp-7.4.13.u46/deploy'
            }
        }
        }
    }
```
