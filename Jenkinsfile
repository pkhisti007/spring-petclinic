pipeline {
    agent any
     options {
        //   clean before build
        skipDefaultCheckout(true)
    }
    tools {
        // Install the Maven version configured as "MAVEN_HOME" and add it to the path.
        maven "MAVEN_HOME"
    }
    
    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', url: 'https://github.com/pkhisti007/spring-petclinic.git'
                //   run Maven on a Windows agent and build the package
                  bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
             stage('upload') {
            steps {
                script
                {
                    //read pom.xml and dynamically get the jar file version number
                    // and upload the artifact to nexus repository
                    // A webhook is configured on the github account which triggers the build on Jenkins
                    
                def mavenPom = readMavenPom file: 'pom.xml'
                   nexusArtifactUploader artifacts: [
                       [
                           artifactId: 'spring-petclinic',
                           classifier: '',
                           file: "target/spring-petclinic-${mavenPom.version}.jar",
                           type: 'jar'
                           ]
                       ],
                       credentialsId: 'c92491ba-c065-4aed-b14b-1adb2b5c5e5a',
                       groupId: 'org.springframework.samples',
                       nexusUrl: '127.0.0.1:8081',
                       nexusVersion: 'nexus3', 
                       protocol: 'http', 
                       repository: 'maven-releases', 
                       version: "${mavenPom.version}"
                  }
            }
           
        }
    }
}
