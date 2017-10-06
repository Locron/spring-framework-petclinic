node {
    stage('Preparation') {
        echo 'Preparation...'
        git 'https://Locron@github.com/Locron/spring-framework-petclinic.git'
    }
    stage('Build') {
        echo 'Build'
        withMaven(jdk: 'jdk1.8', maven: 'maven 3.5') {
            bat "mvn clean package"
        }
        junit '**/target/surefire-reports/TEST-*.xml'
        archiveArtifacts 'target/*.war'
    }
    stage('Quality') {
        echo 'Quality'
        withMaven(jdk: 'jdk1.8', maven: 'maven 3.5') {
            bat "mvn checkstyle:checkstyle pmd:pmd compile sonar:sonar"
        }
        checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '**/checkstyle-result.xml', unHealthy: ''
        pmd canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '**/pmd.xml', unHealthy: ''
    }
    stage('Deploy to Nexus') {
        echo 'Deploy to Nexus'
        withMaven(jdk: 'jdk1.8', maven: 'maven 3.5') {
            bat "mvn clean install deploy:deploy -DaltDeploymentRepository=nexus::default::http://localhost:8081/repository/maven-releases/"
        }
    }
    stage('Deploy to Tomcat') {
        echo 'Deploy to Tomcat'
        withMaven(jdk: 'jdk1.8', maven: 'maven 3.5') {
            bat "mvn tomcat7:redeploy -Dmaven.tomcat.url=http://localhost:10080/manager/text -Dtomcat.username=tomcat -Dtomcat.password=tomcat"
        }
    }
}
