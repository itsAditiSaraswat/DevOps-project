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
`vim docker-compose.yml`
![5](https://user-images.githubusercontent.com/102405945/214836623-9ed301c7-b2f4-4246-a2e3-f86b8b2691d5.png)
`docker-compose up`
![6](https://user-images.githubusercontent.com/102405945/214836692-f1fcb346-7363-4ea3-929a-1055777a2530.png)
![4](https://user-images.githubusercontent.com/102405945/214836707-21f8402c-83fe-458b-930c-0acc85c5ad19.png)
