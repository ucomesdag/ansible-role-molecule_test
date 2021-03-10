#!/usr/bin/env groovy

def container_images = [
  "alpine:latest",
  "amazonlinux:latest",
  "centos:7",
  "centos:latest",
  "debian:latest",
  "debian:bullseye",
  "dedora:32",
  "fedora:latest",
  "opensuse:latest",
  "ubuntu:latest",
  "ubuntu:bionic"]

def branches = ["main"]

def createPipelineTriggers() {
  if (env.BRANCH_NAME !== 'main') {
    // Run every 15 minutes by default
    return [pollSCM('H/15 * * * *')]
  }
}

def get_stage(container_image) {
  stages = {
    image(container_image).inside {
      def str = container_image.split(':')
      stage("test (${str[0]}, ${str[1]})") {
          echo 'molecule test'
      }
    }
  }
  return stages
}

node('master') {

  properties([
      pipelineTriggers(createPipelineTriggers()),
      // disableConcurrentBuilds(),
      buildDiscarder(logRotator(numToKeepStr: '10'))
  ])

  image(container_image).inside {
    stage('lint') {
        echo 'linting'
    }
  }

  def stages = [:]
  for (int i = 0; i < container_images.size(); i++) {
      def container_image = container_images[i]
      stages[container_image] = get_stage(container_image)
  }
  parallel stages
}
