#! /usr/bin/env groovy
import hudson.model.*
import hudson.EnvVars
import java.net.URL

node{
  stage("git checkout"){
    git "https://github.com/sauravchaudhary43/webapp.git"
  }
  stage("compile addressbook"){
    withMaven(jdk: 'java-1.8', maven: 'Maven3.6') {
        sh "mvn compile"
    }
  }
  stage("pmd coderwview"){
    withMaven(jdk: 'java-1.8', maven: 'Maven3.6') {
      sh "mvn -P metrics pmd:pmd"
      pmd canComputeNew: false, defaultEncoding: '', healthy: '', pattern: 'target/pmd.xml', unHealthy: ''
    }
  }
  stage("Junit testing"){
    withMaven(jdk: 'java-1.8',maven: 'Maven3.6'){
      sh "mvn test"
      junit 'target/surefire-reports/*.xml'
    }
  }
  stage("Cubebrtura testing"){
    withMaven(jdk: 'java-1.8',maven: 'Maven3.6'){
      sh "mvn cobertura:cobertura -Dcobertura.report.format=xml"
    }
  }
  stage("package addressbook"){
    withMaven(jdk: 'java-1.8', maven: 'Maven3.6') {
       sh "mvn package"
    }
  }
  sshagent(['deployer']) {
    sh 'scp Package/target/addressbook.war deployer@10.0.43.51:/root/opt/apache-tomcat-8.5.43/webapps'
  }
}
