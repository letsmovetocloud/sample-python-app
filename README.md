code build project 

--> testpython project

description: sample app


source code provider 


authorised with git hub 

git hub

copy git hub url : 

https://github.com/letsmovetocloud/testpythonapp.git

Environment : 

mamaged instance (select this one )     custome image

operating system : ubuntu 

Runtime : standard

iamge: aws/codebuild/standard:7.0 

image version : always use the latest image for thisrun time version 

Environment type : linux 

Create new service role : codebuild-s-servicerole27122023

goto iam 

aws service 

use case : code build

allow code build 

policy 

aws ssm full accesspolicy

note : attach aws systems maanger with code build service role

-------

goto 

system manager 

paramaeter store 

create parameters for 

/testapp/docker-credentials/username

/testapp/docker-credentials/password

/testapp/docker-registry/url

Create parameter 

/testapp/docker-credentials/username

standard

type :

.securestring 

mycurrent account 

use default kms 

value 

attach aws systems maanger with code build service role 



use buidspecfile   insert build commands( select this) 

swith to editor 




version: 0.2

env:
  parameter-store:
    DOCKER_REGISTRY_USERNAME: /myapp/docker-credentials/username
    DOCKER_REGISTRY_PASSWORD: /myapp/docker-credentials/password
    DOCKER_REGISTRY_URL: /myapp/docker-registry/url
phases:
  install:
    runtime-versions:
      python: 3.11
  pre_build:
    commands:
      - echo "Installing dependencies..."
      - pip install -r requirements.txt
  build:
    commands:
      - echo "Running tests..."
      - echo "Building Docker image..."
      - echo "$DOCKER_REGISTRY_PASSWORD" | docker login "$DOCKER_REGISTRY_URL" -u "$DOCKER_REGISTRY_USERNAME" --password-stdin
      - docker build -t "$DOCKER_REGISTRY_USERNAME/simple-python-flask-app:latest" .
      - docker push "$DOCKER_REGISTRY_USERNAME/simple-python-flask-app:latest"
  post_build:
    commands:
      - echo "Build completed successfully!"



once code build project build 

edit enivronments 


check this  Allow aws code build to modify serivice role so it can be use with this build project 


overide iamge 

priviledged 

check this enable this flag if you want to build docker iamges or want to your build elevates 

upadte environment 


once code build executed : 
--------------------------------------------
and docker image push to your docker account create code pipeline  

integarte the code pipeline :

goto code pipeline

create pipeline

sample-pytho-app ( create new service role)

newservice role 

source provider

git hub version2

connect to github 

python-app


repo name 

branch name 

code pipeline default 

build provider 

code build project 

region select the region 

project name 

single build 

skip deployer for now 

create pipeline 

----------------------------------------------------------


goto code deploy 

getting started create application

application name ---sample-python-app

compute platform 

ec2/on-prem

click on create application

---> build one server with ubuntu image 

and create tag for application

dev    pythonprojectwebapp

install the docker.io in server 

---> sudo apt install docker.io -y

install agent (code deploy) in server  

search for code deploy link 

https://docs.aws.amazon.com/codedeploy/latest/userguide/codedeploy-agent-operations-install-ubuntu.html

code deploy agent instance 

sudo apt update 

sudo apt install ruby-full 

sudo apt install wget 

region-code : ap-southeast-2

bucket name region wise :

aws-codedeploy-ap-southeast-2

wget https://aws-codedeploy-ap-southeast-2.s3.ap-southeast-2.amazonaws.com/latest/install

chmod +x ./install

sudo ./install auto

we need to start the service 

sudo service codedeploy-agent status


sudo service codedeploy-agent start



----------------------------
go to iam 

create code deploy role and assign to ec2 

--> select aws service 

--- > code deploy 

ec2tocodedeployrole


after assign the code deploy role restart code deploy services 

restart services 

>sudo service codedeploy-agent restart

-----------------------------------

create deployemnt group 

deployment group name 

sample-python-app

ec2tocodedeployrole --same which you allocated to servers 

-->AwscodeDeployrole

-->AmazonEc2fullaccess 

Deployment-type

in-place 

select Amazon EC2 intances 

select  the tag 

dont require load balancer 

create deployement group 

------------------------------------

goto applications

create deployement 

deployement groups select


git hub (appspec.yaml file required in git hub root account)

authenticate 

reposiroty name 

letsmovetocloud/sample-pytho-app

commit id (to test) 

create deployment 


---------------------------

after all the changes done 
if the deploy will fail install docker in ubuntu server

sudo apt install docker.io -y  


------------------------------

integragte code deploy with code pipeline 


click on edit 

+ Add stage 

code-deploy 

click on action group 

Action name 

code-deploy 

Action provider 

AWS CodeDeploy 

Region 

Sydeny 

input artifacts 

-->build artifacts 

choose application name 

sample-python-app

choose deployment group 

sample-python-app

done 

























































 
















