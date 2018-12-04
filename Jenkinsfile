node('maven') {
             // define commands
             def mvnCmd = "mvn -s nexus3_settings.xml"
             stage ('Build') {
               git branch: 'master', url: 'https://github.com/oniabifo/openshift3mlbparks.git'
               sh "${mvnCmd} clean install -DskipTests=true"
             }
             stage ('Test and Analysis') {
               parallel (
                   'Test': {
                       sh "${mvnCmd} test"
                       step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
                   },
                   'Static Analysis': {
                       sh "${mvnCmd} jacoco:report sonar:sonar -Dsonar.host.url=http://sonarqube-abi-sonarqube.apps.na39.openshift.opentlc.com -DskipTests=true"
                   }
               )
             }
}