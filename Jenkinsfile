#!/usr/bin/groovy
@Library('github.com/fabric8io/fabric8-pipeline-library@master')
def utils = new io.fabric8.Utils()
def repo = 'caddy-image'
def org = 'fabric8io-images'
dockerTemplate{
    clientsNode{
        checkout scm
        if (utils.isCI()){
            echo 'No CI tests to run'
            
        } else if (utils.isCD()){
            echo 'Running CD pipeline'
            sh "git remote set-url origin git@github.com:${org}/${repo}.git"
            def newVersion = getNewVersion {}

            stage 'tag'
            container('clients') {
                gitTag{
                    releaseVersion = newVersion
                    skipVersionPrefix = true
                }
            }

            container('docker') {
                stage ('build'){
                    sh "docker build -t fabric8/caddy:${newVersion} ."
                }
                
                stage ('push to dockerhub'){
                    sh "docker push fabric8/caddy:${newVersion}"
                }
            }            
        }
    }
}
