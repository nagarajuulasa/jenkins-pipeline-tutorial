// Declarative pipelines must be enclosed with a "pipeline" directive.
pipeline {
    // This line is required for declarative pipelines. Just keep it here.
    agent any
    // This section contains environment variables which are available for use in the
    // pipeline's stages.
    environment {
	    region = "us-east-1"
        docker_repo_uri = "193804096296.dkr.ecr.us-east-1.amazonaws.com/sample-app"
		task_def_arn = ""
        cluster = ""
        exec_role_arn = ""
    }
    // Here you can define one or more stages for your pipeline.
    // Each stage can execute one or more steps.
    stages {
        // This is a stage.
        stage('Build') {
          steps {
        // Get SHA1 of current commit
        script {
            commit_id = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
        }
        // Build the Docker image
        sh "docker build -t ${docker_repo_uri}:${commit_id} ."
        // Get Docker login credentials for ECR
        sh "aws ecr get-login --no-include-email --region ${region} | sh"
        // Push Docker image
        sh "docker push ${docker_repo_uri}:${commit_id}"
        // Clean up
        sh "docker rmi -f ${docker_repo_uri}:${commit_id}"
       }
      }
    }
}
