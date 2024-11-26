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

</br> To create the flask app as a file use commands...
</br><b>commands</b>
</br><i> nano backend/app.py</i>

</br>When nano editor opens type in..

</br><i>from flask import Flask
</br>app = Flask(__name__)

</br>@app.route("/")
</br>def home():
</br>   return "Hello from Backend Server!"</i>

</br> The above code import flask module. Creates an instance of the Flask class. The code will then assign the "/" symbol as the route for the app. A function called "home" will return the message "Hello from Back Server!" to signal we have reached the backend server later in the project.

</br> Save the modified buffer and exit the file.

</br>![Screenshot (314)](https://github.com/user-attachments/assets/a41a5e6d-4443-4fa8-b821-ba6dcb372ab3)



</br>4. Build the backend Docker image:

</br> We will now build the backend Docker image utilizing the command..
</br><i>cd backend (if not already there)
</br>docker build -t backend-server . </i>

</br> ***Note: Dont forget the period on the second command***

</br>![Screenshot (316)](https://github.com/user-attachments/assets/60ce682b-88cc-4f91-a65a-6c81651cc587)

</br![Screenshot (315)](https://github.com/user-attachments/assets/18766ae9-ecec-46ac-98ee-1167fb8e6197)


<h2>Deploy Backend Servers</h2>
</br>1. Create Docker Compose file for backend servers:
</br> We will first want to cd into the ~/nginx-docker-loadbalancer directory and create a file via the nano command...
</br><b>Commands:</b>
</br><i>cd ..
<br>nano docker-compose.yml</i>

</br>You will then need edit the file so that it creates two Docker Composes file for backend servers, save then exit file...

</br>***Note: Take note of proper indentation and format of text, if ERROR occurs it may be do to this or version; should update "Docker Compose"***

</br>![Screenshot (321)](https://github.com/user-attachments/assets/c5d16f6a-7c6d-4a66-98a5-dd947c0e4e22)

</br>


</br> We will now start the backend servers and verify the backend servers are running with the commands...
</br><b>Commands:</b>
</br><i>docker-compose up -d 
</br>curl http://<vm_IP>:8081
</br>curl hhtp://<vm_IP>:8082</i>

</br> the "docker-compose up -d" command tell docker the start the backend servers where the containers are, the -d flag stands for detached mode. This tells the containers to run in the background.

</br>![Screenshot 2024-11-26 131728](https://github.com/user-attachments/assets/a074ba5a-9221-4006-83b9-320aec25e5ba)


<h2>Configure NGINX as Load Balancer</h2>
</br>1. Create and change into the directory for NGINX using the commands...
</br><b>Commands:</b>
</br><i> mkdir nginx && cd nginx</i>

</br>![Screenshot (324)](https://github.com/user-attachments/assets/cd0759bd-9b21-458b-a40d-dbdd102d5e51)

</br>2. Now write the NGINX configuration file as follows.
</br><b>Commands:</b>
</br><i>nano nginx.conf</i>

</br>![Screenshot (325)](https://github.com/user-attachments/assets/cbb430a8-51e7-4726-a375-e3100c3946ed)

</br>CONFIGURATION EXPLANATION
</br>


</br>3. Add WAF configuration with the following commands
</br><b>Commands:</b>
</br><i>mkdir -p /etc/nginx/modsec
</br>nano /etc/nginx/modesec/main.conf</i>

</br>To the main.conf, write the following OWASP CRS or other basic rules...
</br><i>Include /etc/nginx/modsec/coreruleset/crs-setup.conf</i>
</br><i>Include /etc/nginx/modsec/coreruleset/rules/*.conf</i>

</br>![Screenshot 2024-11-26 134103](https://github.com/user-attachments/assets/ba3a1372-1723-4ae4-83a1-a21f4a3a3e39)

</br>RULES EXPLANATION


</br>4. Create a Dockerfile for NGINX with ModSecurity
</br>from the ~/nginx-docker-loadbalancer directory, input the command
</br><i>nano nginx/Dockerfile</i>

</br> In the file write the following...
</br><i>FROM nginx:latest
</br>RUN apt-get update && \
</br>    apt-get install -y libmodsecurity-dev && \
</br>    apt-get clean
</br>COPY nginx.conf /etc/nginx/nginx.conf
</br>COPY modsec /etc/nginx/modsec

</br>![Screenshot (326)](https://github.com/user-attachments/assets/5b4d0991-8592-478b-8f3a-d5ddaf078132)


</br>5. We will now build the NGINX imagedocker. From the ~/nginx-docker-loadbalancer directory, input the command:
</br><i>docker build -t nginx-loadbalancer ./nginx</i>

</br>![Screenshot (327)](https://github.com/user-attachments/assets/b1a49f5d-3321-4115-bd1f-3c7d8fd6306a)


<h2>Deploy NGINX load Balancer</h2>
</br>1. Add NGINX to the Docker Compose file. Navigate to the docker-compose.yml, nano into the file and write the following:

</br>![Screenshot 2024-11-26 141237](https://github.com/user-attachments/assets/cac47ef6-5135-4f7a-8657-1028fd928a8f)


</br>2.Restart all services using the following commands:
</br><i>docker-compose down
</br>docker-compose up -d</i>


</br>3. Now access the load balancer by using the <i>curl http://<VM_IP></i> command to see the traffic being routed to the backend servers.

</br>


<h2>Testing</h2>
</br>
