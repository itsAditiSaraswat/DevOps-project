# Explore California
*DevOps Project*

---

| &nbsp; | &nbsp; |
| --- | ----------- |
| **Objective and Approach** | - Bridge the gap between production and development by containerizing the website ‘Explore California’ with Docker and Docker Compose<br>- Unit and Integration testing using RSpec, Capybara and Selenium<br>- Rapidly deploy a working instance of the website into the cloud (AWS S3) using Terraform<br>- Build and deploy the website using CICD pipeline with Jenkins |
| **Impact** | - All environments consistent using Docker/ Docker Compose</br>- Deploys into production automated via Terraform</br>- Production deploys 100% automated through CI|
<br>

**Primary Technology:** Docker, Docker Compose, RSpec, Capybara, Selenium, AWS S3, Terraform, Jenkins
<br><br>


<br><br>
---

## 1. Testing Locally with Docker

### 1.1 Installing Docker
`sudo apt-get install docker-engine -y`

### 1.2 Writing Dockerfile
`vim Dockerfile`
![1](https://user-images.githubusercontent.com/102405945/214835747-74afa794-87e0-4443-9836-5c4feaafa175.png)
`docker build --tag website .`
![2](https://user-images.githubusercontent.com/102405945/214835949-b925dd1d-4477-4876-a464-c2332372fe63.png)
`docker run --publish 80:80 website`
![3](https://user-images.githubusercontent.com/102405945/214836098-1f5ecbfe-8bec-4225-b3a4-986792338508.png)
![4](https://user-images.githubusercontent.com/102405945/214836107-e3ee822c-a7e3-4907-980b-7e7dac083b7a.png)

### 1.2 Writing Docker Compose manifest
`vim docker-compose.yml`o
![5](https://user-images.githubusercontent.com/102405945/214836623-9ed301c7-b2f4-4246-a2e3-f86b8b2691d5.png)
`docker-compose up`
![6](https://user-images.githubusercontent.com/102405945/214836692-f1fcb346-7363-4ea3-929a-1055777a2530.png)
![4](https://user-images.githubusercontent.com/102405945/214836707-21f8402c-83fe-458b-930c-0acc85c5ad19.png)



## 2. Testing the App with RSpec, Capybara, and Selenium

To write some automated tests for the website, we will use RSpec, Capybara, and Selenium.

**RSpec** is a Ruby-based testing framework. It is a domain-specific language or DSL. <br>
**Capybara** is a tool that let's you use a web driver to create a browser and interact with the website. <br>
**Selenium** is a web driver that spins up a real web browser and tests against whatever the web browser sees. <br>

### 2.1 Setting up the test
Create a folder spec. Create another folder unit within spec. Now, create a file page_spec.rb
`vim spec/unit/page_spec.rb`
![1](https://user-images.githubusercontent.com/102405945/215341121-c02bbcb0-5d74-4b6d-9de1-a09bf151f53f.png)
![2](https://user-images.githubusercontent.com/102405945/215341128-21842d72-2769-485e-bb14-1f5474e5d6bb.png)

### 2.2 Setting up Docker Compose service
`vim docker-compose.yml`
![3](https://user-images.githubusercontent.com/102405945/215341356-be088395-f33a-42b4-baa4-69a44c584055.png)

### 2.3 Creating the Dockerfile
`vim rspec.docerfile`
![4](https://user-images.githubusercontent.com/102405945/215342122-b5feb02b-667e-4ac6-8060-1c00809491b3.png)
`vim docker-compose.yml`
![5](https://user-images.githubusercontent.com/102405945/215342133-a2b29125-cc1a-4927-bbd7-c4e405603820.png)

### 2.4 Running the test
`docker-compose up -d website` <br>
`docker-compose run -rm unit-tests`
![6](https://user-images.githubusercontent.com/102405945/215343030-af70d852-6303-4a3c-aeee-d1df47e03063.png)
![7](https://user-images.githubusercontent.com/102405945/215343034-5d4508cb-24c1-463e-ab40-7cd778933eac.png)

### 2.5 Identifying and testing an element
`docker-compose config` <br>
`docker-compose up -d website`
![8](https://user-images.githubusercontent.com/102405945/215343521-67b8b914-5ca6-4d2e-a1fb-24fc6fe1c4d1.png)
![9](https://user-images.githubusercontent.com/102405945/215343528-9ed9bea5-b8cf-4a9a-a120-6ae874d41648.png)
`vim page_spec.rb`
![10](https://user-images.githubusercontent.com/102405945/215343540-b4495a2a-846e-43e3-b7fa-85dd72f757dc.png)

### 2.6 Setting up Selenium
`vim page_spec.rb` <br>
`docker-compose run -rm unit-tests`
![11](https://user-images.githubusercontent.com/102405945/215344222-93eea3f6-43a3-4a4c-971c-66b832fd9739.png)

### 2.7 Adding Selenium service to Docker Compose
`vim docker-compose.yml`
![12](https://user-images.githubusercontent.com/102405945/215344574-afad248a-22e0-4578-919e-d6181f409c7f.png)
`docker-compose up -d website selenium`
![13](https://user-images.githubusercontent.com/102405945/215344582-16268af1-39ae-46e2-9670-60e827b3d298.png)

### 2.8 Running the test with Selenium
`docker-compose run --rm unit-tests`
![14](https://user-images.githubusercontent.com/102405945/215344859-b8b1543d-42f8-47be-80a4-6eb14d38e829.png)


## 3. Infrastructure as code with Terraform

### 3.1 Creating the Terraform Dockerfile
`vim terraform.Dockerfile`
![1](https://user-images.githubusercontent.com/102405945/215345670-a14cc7a9-1a38-443d-babc-d207b0a7062d.png)
`docker build -t terraform .` <br>
`docker build -t terraform . -f terraform.Dockerfile` <br>
`docker run --rm --interactive --tty --entrypoint sh terraform` <br>
![2](https://user-images.githubusercontent.com/102405945/215345676-212cc641-562c-41ef-a414-02eb7afca560.png)

### 3.2 Building and Testing a Terraform Docker image
`docker build --tag terraform --file terraform.Dockerfile .` <br>
`docker run --rm terraform` <br>
![3](https://user-images.githubusercontent.com/102405945/215531365-58e35f58-b140-4b47-a79f-5fcfba7e6c20.png)

### 3.3 Creating a Terraform Docker Compose service
`vim docker-compose.yml` <br>
`docker-compose build terraform` <br>
`docker-compose run --rm terraform` <br>
![4](https://user-images.githubusercontent.com/102405945/215531692-06a0095b-25a4-4dc9-b9f0-9c0b8f4e3026.png)
![5](https://user-images.githubusercontent.com/102405945/215531709-8d3b391e-732d-43d9-936e-827a5d65417a.png)
![6](https://user-images.githubusercontent.com/102405945/215531733-fea476b1-d932-4ec1-aeb7-04595721aef8.png)

### 3.4 AWS deployment by writing Terraform code
`vim main.tf`
![7](https://user-images.githubusercontent.com/102405945/215531884-6c5a3bca-ae78-42ca-aeb9-7e5e2ed75573.png)

### 3.5 Reviewing and Applying Terraform plan
`docker-compose run --rm terraform init` <br>
`vim docker-compose.yml` To add AWS access keys <br>
`docker-compose run --rm terraform plan` <br>
![8](https://user-images.githubusercontent.com/102405945/215532184-c2fef23a-bf70-4c3b-8ef8-7dd16f39fee9.png)
![9](https://user-images.githubusercontent.com/102405945/215532206-a3221ee0-4279-4653-84af-591e0198a98c.png)
![10](https://user-images.githubusercontent.com/102405945/215532230-734e5140-35a2-495c-a225-f9e13e8036ec.png)

### 3.6 Deploying the website into AWS S3
`docker-compose run --rm aws` <br>
`docker-compose run --rm --entrypoint aws aws ec2 describe-instances` <br>
`docker-compose run --rm --entrypoint aws aws s3 cp --recursive website/ s3://explorecalifornia.org` <br>
`docker-compose run --rm terraform output` <br>
![11](https://user-images.githubusercontent.com/102405945/215532579-3fe10115-b192-4711-8132-21dc9d9b56e1.png)
![12](https://user-images.githubusercontent.com/102405945/215532600-6d2c629f-720d-4f95-971c-724a1cb07110.png)
![13](https://user-images.githubusercontent.com/102405945/215532614-c6ea1ad2-2437-45ae-93e9-095a28e1a102.png)

### 3.7 Writing integration test
`vim page_spec.rb` <br>
`docker-compose run --rm terraform plan` <br>
`docker-compose run --rm terraform apply` <br>
`vim docker-compose.yml` <br>
![14](https://user-images.githubusercontent.com/102405945/215532914-d9322582-ab23-4781-9600-c1aa700edb4f.png)
![15](https://user-images.githubusercontent.com/102405945/215532942-854c7fda-2d8b-4c58-bc64-28936ab69f92.png)
![16](https://user-images.githubusercontent.com/102405945/215532975-ac99ac73-ff91-45bc-b6b0-0e65c6e505e4.png)
![17](https://user-images.githubusercontent.com/102405945/215532996-b1041f95-3d88-4661-9e42-87b35374280e.png)

### 3.8 Running integration test
`docker-compose up -d selenium` <br>
`docker-compose run --rm integration-tests` <br>
`docker-compose run --rm aws aws s3 cp website/ s3://explorecalifornia.org --recursive` <br>
`docker-compose run --rm aws aws s3 r website/ s3://explorecalifornia.org --recursive` <br>
`docker-compose run --rm terraform destroy` <br>
![18](https://user-images.githubusercontent.com/102405945/215533323-d5c4e4b4-f7b9-49b8-9258-39cc78fcf82e.png)
![19](https://user-images.githubusercontent.com/102405945/215533390-0846a470-36d2-433a-be5a-112734e88049.png)
![20](https://user-images.githubusercontent.com/102405945/215533456-cd31e89b-a0e2-4c72-a073-ba8b63d99734.png)
![21](https://user-images.githubusercontent.com/102405945/215533496-1b9d5aa3-058c-4b7f-ad5f-b4a14471365a.png)
![22](https://user-images.githubusercontent.com/102405945/215533512-6bcc5716-dfdb-4008-9b0e-211cc5b4a905.png)



## 4. CI/CD as Code with Jenkins

### 4.1 Installing Jenkins on Docker
`vim jenkins.Dockerfile` <br>
`vim docker-compose.yml` <br>
`docker-compose up jenkins` <br>
![1](https://user-images.githubusercontent.com/102405945/215823999-4a8dfa81-300f-4e48-ba6c-94ff207a9a70.png)
![2](https://user-images.githubusercontent.com/102405945/215824015-40939c00-e3e6-4552-b568-3900c52ede44.png)
![3](https://user-images.githubusercontent.com/102405945/215824071-c0726c96-e7bc-4211-9627-3671f26b2edf.png)
![4](https://user-images.githubusercontent.com/102405945/215824089-304f9d4d-615b-4dde-8cd2-6719b83005f2.png)
![5](https://user-images.githubusercontent.com/102405945/215824128-b6d05230-da23-439b-803f-c5059ee3d7c4.png)

### 4.2 Writing a Jenkinsfile for the app
`vim jenkinsfile`
![6](https://user-images.githubusercontent.com/102405945/215824147-327c8094-e17a-4550-95ad-34125de8a4d4.png)

### 4.3 Using Jenkinsfile to deploy the app
`git init` <br>
`git checkout -b master` <br>
`git add -A` <br>
`git commit -m "Initialize repository"` <br>
`docker-compose up jenkins` <br>
![7](https://user-images.githubusercontent.com/102405945/215824251-266770a1-a707-4630-8f3c-6389192f0196.png)
![8](https://user-images.githubusercontent.com/102405945/215824283-9818f1b8-aaa7-4bf1-b1d6-89534528b2c3.png)
![9](https://user-images.githubusercontent.com/102405945/215824326-37294898-efa3-43cd-a3e3-dc49f881b49d.png)
![10](https://user-images.githubusercontent.com/102405945/215824364-80f00e57-527a-4887-9fa6-824dfd213e4d.png)
![11](https://user-images.githubusercontent.com/102405945/215824387-b367bdf7-cb25-42b3-87b0-4ea81dfb2ee1.png)
![12](https://user-images.githubusercontent.com/102405945/215824423-3d9e7b4b-9915-403d-b749-7aaa243a346b.png)
![13](https://user-images.githubusercontent.com/102405945/215824448-36406960-1b2d-4cbb-a15f-bb26863d9e3a.png)
