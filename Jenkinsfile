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
    withMaven(jdk: 'java-1.8',maven: 'MAven3.6'){
      sh "mvn -P metrics pmd:pmd"
      pmd canComputeNew: false, defaultEncoding: '', healthy: '', pattern: 'target/pmd.xml', unHealthy: ''
    }
  }
  stage("package addressbook"){
    withMaven(jdk: 'java-1.8', maven: 'Maven3.6') {
       sh "mvn package"
    }
  }
}
