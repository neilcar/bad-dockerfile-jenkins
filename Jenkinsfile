Skip to content
This repository
Search
Pull requests
Issues
Marketplace
Explore
 @neilcar
 Sign out
 Unwatch 1
  Star 0  Fork 0 neilcar/docker-test
 Code  Issues 0  Pull requests 0  Projects 0  Wiki  Insights  Settings
Branch: master Find file Copy pathdocker-test/Jenkinsfile
f2181c6  on Nov 9
@neilcar neilcar Update Jenkinsfile
1 contributor
RawBlameHistory     
45 lines (35 sloc)  1.42 KB
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
        twistlockScan ca: '', cert: '', compliancePolicy: 'warn', dockerAddress: 'tcp://172.17.0.1:2375', ignoreImageBuildTime: false, image: 'neilcar*', key: '', logLevel: 'true', policy: 'warn', requirePackageUpdate: false, timeout: 10
    }
    
    stage('Publish scan results') {
        twistlockPublish ca: '', cert: '', dockerAddress: 'tcp://172.17.0.1:2375', image: 'neilcar*', key: '', logLevel: 'true', timeout: 10
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
© 2017 GitHub, Inc.
Terms
Privacy
Security
Status
Help
Contact GitHub
API
Training
Shop
Blog
About
