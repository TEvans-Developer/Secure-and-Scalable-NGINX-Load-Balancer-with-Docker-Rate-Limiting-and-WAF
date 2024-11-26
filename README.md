cl# Secure-and-Scalable-NGINX-Load-Balancer-with-Docker-Rate-Limiting-and-WAF
This project demonstrates how to build a secure and scalable NGINX load balancer will utilizing Docker for containerization, rate limiting and WAF for security.

<h2>Project Overview:</h2>
</br>A single VM to host both load balancer and the backend servers using Docker containers.

</br>NGINX as the Load Balancer. Will be configured with rate limiting and a Web application Firewall (WAF).

</br>Docker will run the backend servers and NGINX as isolated services.

<hr>

<h2>Requirements:</h2>
</br> Virtual Box
</br> Server: Ubuntu Server 24.04.01 LTS  
</br> Load Balancer: NGINX (VERSION)**
</br> Container: Docker (VERSION)**

<hr>

<h2>Prerequisites</h2>
</br>1.Install Docker and Docker Compose on the Ubuntu VM:
</br> <b>Commands:</b>
</br> <i>sudo apt update 
</br> sudo install -y docker.io-compose  
</br> sudo systemctl enable docker</i>

</br> *** Note: If required to be root user, use command, "sudo su" in place of sudo ***

</br>![Screenshot (309)](https://github.com/user-attachments/assets/d7248dbf-fc1b-45e8-96f7-be9ae400e352)



<h2>Set up Backend Servers</h2>
</br>1. Create a directory for the project.

</br><b>Commands:</b>
</br><i>mkdir ~/nginx-docker-loadbalancer && cd ~/nginx-docker-loadbalancer</i>

</br> Creates a directory call /nginx-dock-loadbalancer in the root and changes into the directory that was just created.

</br>![Screenshot (310)](https://github.com/user-attachments/assets/bd61f55a-48fd-40b9-a478-ac4cde142508)



</br>2. Create a Dockerfile for a simple backend server:

</br><b>Commands:</b>
</br><i>mkdir backend && nano backend/Dockerfile </i>

</br> The above command will back a directory named "backend".
</br> The "&&" will signal to create and open a file named "Dockerfile" within the "backend" directory after the directroy is created.

</br>![Screenshot (311)](https://github.com/user-attachments/assets/7790bfa4-4aea-42b8-85e6-41375de9eed3)

</br>In the file write...
</br><i> FROM python:3.9-slim
</br>WORKDIR /app
</br>COPY . .
</br>RUN pip install flask
</br>CMD ["python", "-m", "flask", "run", "--host=0.0.0.0", "--port==8080"]</i>

</br> save and exit file.

</br>![Screenshot (312)](https://github.com/user-attachments/assets/d8df4ec4-50e9-4e3b-8a82-520cb04d16c6)



</br>3.Add a basic flask app:
</br><b>command
