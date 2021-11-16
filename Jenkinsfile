def imageName = 'stephnangue/movies-loader'
def registry = '982039600869.dkr.ecr.eu-central-1.amazonaws.com/stephnangue/movies-loader'
def credentials = 'ecr:eu-central-1:jenkins_aws_id'

node('workers'){
    stage('Checkout'){
        checkout scm
    }
    

    stage('Unit Tests'){
        def imageTest= docker.build("${imageName}-test", "-f Dockerfile.test .")
        imageTest.inside{
            sh "python test_main.py"
        }
    }

    stage('Build'){
        docker.build(imageName)
    }

    stage('Push'){
        docker.withRegistry("https://"+"${registry}","${credentials}") {
            docker.image(imageName).push(commitID())

            if (env.BRANCH_NAME == 'develop') {
                docker.image(imageName).push('develop')
            }
        }    
    }

    stage('Deploy'){
        if(env.BRANCH_NAME == 'develop'){
            build job: "watchlist-deployment/${env.BRANCH_NAME}"
        }
    }
    


/*
    stage('Analyze'){
        def scannedImage = "${registry}:${commitID()} ${workspace}/Dockerfile"
        writeFile file: 'images', text: scannedImage
        anchore name: 'images'
    }
*/
}

def commitID() {
    sh 'git rev-parse HEAD > .git/commitID'
    def commitID = readFile('.git/commitID').trim()
    sh 'rm .git/commitID'
    commitID
}