node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        git url: 'https://github.com/ianmiell/bad-dockerfile'
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("demo/bad-dockerfile")
    }

    stage('Scan image') {
        twistlockScan ca: '', cert: '', dockerAddress: 'unix:///var/run/docker.sock', compliancePolicy: 'warn', ignoreImageBuildTime: false, image: 'demo/bad-dockerfile:latest', key: '', logLevel: 'true', policy: 'warn', requirePackageUpdate: false, timeout: 10
    }
    
    stage('Publish scan results') {
        twistlockPublish ca: '', cert: '', dockerAddress: 'unix:///var/run/docker.sock', image: 'demo/bad-dockerfile:latest', key: '', logLevel: 'true', timeout: 10
    }
    
    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('http://172.17.0.1:5000') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
