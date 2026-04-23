docker --version

cd /home/project

[ ! -d 'CC201' ] && git clone https://github.com/ibm-developer-skills-network/CC201.git

cd CC201/labs/1_ContainersAndDocker/

docker images

docker pull hello-world

docker images

docker run hello-world

docker ps -a

docker container rm <container_id>

docker ps -a

docker build . -t myimage:v1

docker images

docker run -dp 8080:8080 myimage:v1

curl localhost:8080

docker stop $(docker ps -q)

docker ps




# This one is for IBMcloud
ibmcloud target

# Create an IBM namespace
ibmcloud cr namespaces

# Set region
ibmcloud cr region-set us-south

ibmcloud cr login

# Export the namespace and the environment
export MY_NAMESPACE=sn-labs-$USERNAME

docker tag myimage:v1 us.icr.io/$MY_NAMESPACE/hello-world:1

docker push us.icr.io/$MY_NAMESPACE/hello-world:1

ibmcloud cr images
ibmcloud cr images --restrict $MY_NAMESPACE
