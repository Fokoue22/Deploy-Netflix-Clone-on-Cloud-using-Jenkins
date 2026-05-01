# Deploy-Netflix-Clone-on-Cloud-using-Jenkins
Embarking on an exciting DevSecOps journey, we're diving into the deployment of a Netflix  Clone on the cloud using Jenkins. This project encapsulates the fusion of development,  security, and operations practices, ensuring a streamlined and secure pipeline for delivering  software. 

### Project Architecture Review:
![Alt text](images/Project-Architecture.jpg)


### **Phase 1: Initial Setup and Deployment**

**Step 1: Launch EC2 (Ubuntu 22.04):**

- Provision an EC2 instance on AWS with Ubuntu 22.04.
- Add t2-large
- Uses your defaut VPC and create a new SG that Allow SSH, HTTPS and HTTP for now 
- Add arrount 25 GiB of storage due to alot of different blocking we'are going to use. 
- Launche the EC2 instance
- Before connecting using SSH create and associate an Elastic IP to the new EC2 instance
- Connect to the instance using SSH by "EC2 Instance Connect" or "MobaXterm"
- If you have any error make sur the port 22 in your Security Group is open 

**Step 2: Clone the Code:**

- Update all the packages with the command below and then clone the code.
    ```bash
 
    sudo apt-get update
    ```
- Clone your application's code repository onto the EC2 instance:
    
    ```bash
    git clone https://github.com/Fokoue22/Deploy-Netflix-Clone-on-Cloud-using-Jenkins.git
    ```
- Verifie if the project is in your EC2 instance
    ```bash
 
    cd DevSecOps-NetflixProject/ 
    ls
    ``` 

**Step 3: Install Docker and Run the App Using a Container:**

- Set up Docker on the EC2 instance:
    
    ```bash
    
    sudo apt-get update
    sudo apt-get install docker.io -y
    sudo usermod -aG docker $USER  # Replace with your system's username, e.g., 'ubuntu'
    newgrp docker
    sudo chmod 777 /var/run/docker.sock
    ```
    
- Build and run your application using Docker containers:
    
    ```bash
    docker build -t netflix .
    docker run -d --name netflix -p 8081:80 netflix:latest
    
    #to delete
    docker stop <containerid>
    docker rmi -f netflix
    ```

It will show an error cause you need API key

**Step 4: Get the API Key:**

- Open a web browser and navigate to TMDB (The Movie Database) website.
- Click on "Login" and create an account.
- Once logged in, go to your profile and select "Settings."
- Click on "API" from the left-side panel.
- Create a new API key by clicking "Create" and accepting the terms and conditions.
- Provide the required basic details and click "Submit."
- You will receive your TMDB API key.

Now recreate the Docker image with your api key:
```
docker build --build-arg TMDB_V3_API_KEY=<your-api-key> -t netflix .
```