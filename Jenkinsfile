#!/usr/bin/env groovy

import net.smartcosmos.BuildUtil

@Library('smartcosmos@v1.0.0')

node {
  def helper = new net.smartcosmos.helper()
  def utils = new BuildUtil(env, steps)

  deleteDir()

  stage("Checkout") {
    checkout scm
  }

  def commitId = utils.getCommitId()
  def image

  stage("build") {
    image = docker.build "smartcosmos/exp-build-env"
  }

  if (env.BRANCH_NAME == "master") {
    stage("publish") {
      docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
        image.push(commitId)
        image.push("master")
      }
    }
  }

  // remove the docker image locally
  sh "docker rmi ${image.id}"
}
