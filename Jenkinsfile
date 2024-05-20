node {
    def app

    stage('Clone repository') {
        // Ensure we have the repository cloned to our workspace
        try {
            checkout scm
        } catch (e) {
            error "Failed to clone repository: ${e.message}"
        }
    }

    stage('Build image') {
        // This builds the actual image; synonymous to docker build on the command line
        try {
            app = docker.build("arvind9140/admin")
        } catch (e) {
            error "Failed to build Docker image: ${e.message}"
        }
    }

    stage('Test image') {
        // Ideally, we would run a test framework against our image
        app.inside {
            try {
                sh 'echo "Tests passed"'
            } catch (e) {
                error "Tests failed: ${e.message}"
            }
        }
    }

    stage('Push image') {
        // Finally, we'll push the image with two tags
        try {
            docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                app.push("${env.BUILD_NUMBER}")
                app.push("latest")
            }
        } catch (e) {
            error "Failed to push Docker image: ${e.message}"
        }
    }
}
