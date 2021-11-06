def imageName = 'stephnangue/movies-loader'

node('workers'){
    stage('Checkout'){
        checkout scm
    }

    stage('Unit Tests'){
        def imageTest= docker.build("${imageName}-test", "-f Dockerfile.test .")
         imageTest.inside{
            sh 'python test_main.py'
        }
        sh "docker run --rm -v $PWD/reports:/app/reports ${imageName}-test"
        junit "$PWD/reports/*.xml"
    }

}