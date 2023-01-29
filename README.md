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
