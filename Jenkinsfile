#!/usr/bin/env groovy

// def platforms = [
//   "alpine:latest",
//   "amazonlinux:latest",
//   "archlinux:latest",
//   "centos:7",
//   "centos:latest",
//   "debian:latest",
//   "fedora:32",
//   "fedora:latest",
//   "fedora:rawhide",
//   "opensuse:latest",
//   "ubuntu:latest",
//   "ubuntu:bionic",
//   "ubuntu:xenial"]

def platforms = [
  "fedora:32",
  "fedora:latest"]

def worker_image = "quay.io/ucomesdag/github-action-molecule:latest"

def runParallel = false

node('master') {

  if (env.BRANCH_NAME == 'main') {

    properties([
      pipelineTriggers(createPipelineTriggers()),
      disableConcurrentBuilds(),
      buildDiscarder(logRotator(numToKeepStr: '10'))
    ])

    catchError {

      // slackNotify()

      // Run molecule lint
      stage('Lint') {
        checkout scm
        docker.image(worker_image).inside() {
          sh "molecule lint"
        }
      }
      //

      // Run molecule test on platforms
      def repo = getRepositoryName()
      def stages = [:]
      for (int i = 0; i < platforms.size(); i++) {
        def str = platforms[i].split(':')
        def image = "${str[0]}"
        def tag = "${str[1]}"
        def dest = "../${image}-${tag}/${repo}"
        def n = "${image}-${tag}"
        stages.put(n, prepareStage(image, tag, dest, worker_image))
      }
      if (runParallel) {
        stage('Test'){ parallel(stages) }
      } else {
        for (stage in stages.values()) { stage.call() }
      }
      //

    }
    // slackNotify(currentBuild.result)
  }

}

def prepareStage(String image, String tag, String dest, String worker_image) {
  return {
    stage("Test (${image}, ${tag})") {
      dir(dest) {
        checkout scm
        docker.image(worker_image).inside {
          sh "image=${image} tag=${tag} tox"
        }
        deleteDir()
      }
    }
  }
}

def createPipelineTriggers() {
  if (env.BRANCH_NAME != "main") {
    // Run every 15 minutes by default
    return [pollSCM('H/15 * * * *')]
  }
}

String getRepositoryName() {
    return scm.getUserRemoteConfigs()[0].getUrl().tokenize('/').last().split("\\.")[0]
}

// def slackNotify(String buildStatus = 'STARTED') {
//   // Build status of null means successful.
//   buildStatus = buildStatus ?: 'SUCCESS'
//   // Replace encoded slashes.
//   def decodedJobName = env.JOB_NAME.replaceAll("%2F", "/")
//   // Set colors
//   def color
//   if (buildStatus == 'STARTED') {
//     color = '#D4DADF'
//   } else if (buildStatus == 'SUCCESS') {
//     color = '#BDFFC3'
//   } else if (buildStatus == 'UNSTABLE') {
//     color = '#FFFE89'
//   } else {
//     color = '#FF9FA1'
//   }
//   // Format the message
//   def msg = "${buildStatus}: `${decodedJobName}` #${env.BUILD_NUMBER}: ${env.BUILD_URL}"
//   // Send the message
//   slackSend(color: color, message: msg, channel: env.slack_channel)
// }
