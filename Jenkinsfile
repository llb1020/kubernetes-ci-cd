node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-kenzan"
    registryHost = "harbor.lvshou.com/cicd/"
    imageName = "${registryHost}${appName}:${tag}"
    env.BUILDIMG=imageName
    stage "docker-login"
       sh "docker login -p '!@Q3wa$ESZ' -u devops  harbor.lvshou.com"

    stage "Build"
    
        sh "docker build -t ${imageName} -f applications/hello-kenzan/Dockerfile applications/hello-kenzan"
    
    stage "Push"

        sh "docker push ${imageName}"

    stage "Deploy"

        sh "sed 's#harbor.lvshou.com/cicd/hello-kenzan:latest#'$BUILDIMG'#' applications/hello-kenzan/k8s/deployment.yaml | kubectl apply  -f -"
        sh "kubectl rollout status deployment/hello-kenzan"

}
