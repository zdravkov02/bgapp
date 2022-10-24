pipeline 

{

    agent any
    {
        label 'docker-node'
    }

    stages 

    {
        stage('Create docker network')

        {
            steps
            {
                sh 'docker network ls | grep appnet || docker network create appnet'
            }
        }

        stage('Clone the project') 

        {

            steps 

            {

                sh '''

                    cd /projects

                    if [ -d /projects/bgapp ]; then

                      cd /projects/bgapp

                      git pull http://192.168.99.102:3000/zdravkov/bgapp.git

                    else

                      git clone http://192.168.99.102:3000/zdravkov/bgapp.git

                    fi

                    '''

            }

        }

        stage('Build the Web image')

        {

            steps

            {

                sh 'cd /projects/bgapp && docker image build -t img-web -f Dockerfile.web .'

            }

        }

        stage('Run the Web application')

        {

            steps

            {

                sh '''

                docker container rm -f web || true

                docker container run -d --name web --net appnet -p 8080:80 -v $(pwd)/bgapp/web:/var/www/html:ro img-web

                '''

            }

        }

        stage('Build the DB image')

        {

            steps

            {

                sh 'cd /projects/bgapp && docker image build -t img-db -f Dockerfile.db .'

            }

        }

        stage('Run the DB application')

        {

            steps

            {

                sh '''

                docker container rm -f db || true

                docker container run -d --name db --net appnet -e MYSQL_ROOT_PASSWORD=12345 img-db

                '''

            }

        }

    }

}