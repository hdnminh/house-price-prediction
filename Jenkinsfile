pipeline {
    agent any // Jenkin chon bat ky agent (worker, image) nao available de chay

    options{
        buildDiscarder(logRotator(numToKeepStr: '5', daysToKeepStr: '5')) // khi thuc hien qua nhieu lenh buil
        // gay ton storage de log lai. Noi voi Jenkin de giu lai 5 lan build, 5 ngay gan nhat -? optimize storage,
        // tranh giu lai qua nhieu log info.
        timestamps() // hien ra thoi gian tung cau lenh cua jenkins chay
    }

    environment{
        registry = 'hdnminh/house-price-prediction-api-2'
        registryCredential = 'dockerhub'      
    }

    stages {
        stage('Test') {
            agent { // chon agent nao de chay stage nay
                docker { // chon docker image nao de chay stage nay
                    image 'python:3.8' 
                }
            }
            steps { // cac lenh thuc hien trong stage nay
                echo 'Testing model correctness..'
                sh 'pip install -r requirements.txt && pytest'
            }
        }
        // stage('Test') {
        //     steps {
        //         parallel( // chay nhieu lenh cung luc
        //             test1: {
        //                 echo "Testing something 1 ..."
        //             },
        //             test2: {
        //                 echo "Testing something 2 ..."
        //             }
        //         )
        //     }
        // }
        stage('Build') { // stage build
            steps { // cac lenh thuc hien trong stage nay
                script { // chay script
                    echo 'Building image for deployment..'
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"  // tao image moi voi tag la build number
                    echo 'Pushing image to dockerhub..'
                    docker.withRegistry( '', registryCredential ) { // push image len dockerhub
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
        stage('Deploy') {// stage deploy
            steps { // cac lenh thuc hien trong stage nay
                echo 'Deploying models..'
                echo 'Running a script to trigger pull and start a docker container'
            }
        }
    }
}