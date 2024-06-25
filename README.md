# $${\color{white} \textbf{Project}: \textbf{Angular}  \ \textbf{App}}$$

### Tech Stack
- Frontend: Angular,Nodejs
- Backend: Java-Springboot
- Database: Mariadb
- Cloud:AWS:ec2,s3,cloudfront,rds,route-53,certificate manager
### Summary:
- Launch 3 ec2 instances for Frontend, Backend, and Database respectively
- step1: Set Up Database Instance
- step2: Set Up Backend
- step3: Set UP Frontend
  
# Set Up Database Installing MariaDB, Setting Password, and Importing Database on Ubuntu Linux

# Create RDS Instance.

![db](https://github.com/abhipraydhoble/Project-Angular-App/assets/122669982/8d992b33-4a08-4a68-95ab-1021c1111791)


```bash
sudo apt update
sudo apt install mariadb-server
sudo systemctl start mariadb
sudo systemctl enable mariadb

```
# Take access of RDS. 
```
mysql -h database-1.cx64eie4c86l.ap-south-1.rds.amazonaws.com -u admin -p12345678 
```
# Create Database and give privilleges to User.
```
CREATE DATABASE springbackend;
GRANT ALL PRIVILEGES ON springbackend.* TO 'username'@'localhost' IDENTIFIED BY 'your_password';
```

# Put Schema into Database which we created above.(Note :- Keep springbackend.sql file at same place where we have command fired)
```
mysql -h < your RDS End Point put here > -u admin -p12345678 springbackend < springbackend.sql
```
```bash
sudo mysql -h <Endpoint of your RDS > -u admin -p12345678
```
Check Database here.
```sql
show databases;
use springbackend;
```
Check Table here.
```
show tables;
select * from tbl_workers;
```
# $${\color{white} \textbf{Set} \textbf{Up}  \ \textbf{Angular} \ \textbf{Dockerization}}$$
# You have to give your RDS endpoint,user and password in bellow file 
```
vim ./spring-backend/src/main/resources/application.properties
```
# Docker Build command fire for create Docker Images.(Note :- Dockerfile should present in same dir where all file of backend is present)
```
docker build -t Shan20000/angular:angular_backend .
```
```
docker images
docker run -td -p 8080:8080 <image_id>
```
# In frontend we have mention our backend ip or service name in bellow file (Note:- At the place of localhost)

```
vim ./angular-frontend/src/app/services/worker.service.ts
```
# Docker Build command fire for create Docker Images.(Note :- Dockerfile should present in same dir where all file of backend is present)
# Important:- 
1. In the Case of K8s,We mention our Backend service name in yaml file that name must mention in our Frontend image at the place of localhost then create image & push it on DockerHub
```
docker build -t Shan20000/angular:angular_frontend .
```
```
docker images
docker run -td -p 80:80 <image_id>
```
# Take IP of your instance and Hit it on site.........


# $${\color{white} \textbf{Set} \textbf{Up}  \ \textbf{Angular} \ \textbf{Kubernetes}}$$

# We have to push out frontend and backend images on DockerHub.
# Login with your UserId and Password
```
docker login
```
# Push docker images on DockerHub (Note :- Your repository should be present on your DockerHub)
```
docker push Shan20000/angular:angular_frontend
docker push Shan20000/angular:angular_backend
```
# We have to write YAML file K8s Deployment.

Here is this main.yaml file.

```
apiVersion: v1
kind: Pod
metadata:
  name: frontend-backend-pod
  labels:
    app: frontend-backend
spec:
  containers:
  - name: angular-frontend
    image: rodeaayush240/angular:frontend
    ports:
    - containerPort: 80
      protocol: TCP
  - name: angular-backend
    image: rodeaayush240/angular:backend
    ports:
    - containerPort: 8080
      protocol: TCP
---
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
spec:
  selector:
    app: frontend-backend
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
---
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  selector:
    app: frontend-backend
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 8080
  type: ClusterIP
```
# Important:-
1. Here we mention our Backend service name in yaml file that name must mention in our Frontend image at the place of localhost then create image & push it on DockerHub

# Configured first our EKS Cluster and then fired below commands.
```
  kubectl apply -f main.yaml
```
# Important:- 
1. Loadbalancer Security Group must have port allow 80 8080 and 3306
2. Must check that RDS is Connect to a Claster Node (In which we host our backend)or Not.
# We will get loadbalancer DNS (Hit on Google url)
```
kubectl get svc
```
# Now we attach loadbalancer DNS with Domain name (ocean-learner.cloud) with the help of R53 service of AWS.

Steps:-
  1. Firstly give our register DNS in Hosted Zone (Here we use Hostinger DNS Provider).
  2. We gets 4 NS that Ns we have put in Hostinger as you know it.
  3. Create Record with SubDomain in it (We use here "angular.ocean-learner.cloud") with A Record or CName Record
  4. In case we use A Record that time we use alias. and select our region and loadbalancer
# Now We have hit our DNS On Site.
![angular result](https://github.com/Shantanu20000/Angular_java_Project/assets/163661534/e549e5e7-d0a8-405a-a250-f61f125f20ff)
# Booommmm!! It's Works........



 
