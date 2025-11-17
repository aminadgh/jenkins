Student Management API 📚

Spring Boot + MySQL + Docker + Kubernetes (Minikube)


A lightweight backend service for managing students, using Spring Boot and MySQL, packaged with Docker and deployed on Kubernetes (Minikube).

🚀 Quick Run (Local + Minikube)
1️⃣ Build the Docker image
docker build -t student:latest .

2️⃣ Load into Minikube
eval $(minikube -p minikube docker-env)
minikube image load student:latest

3️⃣ Deploy
kubectl apply -f k8s-deployment.yaml

4️⃣ Check status
kubectl get pods

5️⃣ Open service
minikube service student-app-service

🔗 REST API Endpoints
Method	Endpoint	Description
GET	/students/getAllStudents	Get all students
GET	/students/getStudent/{id}	Get student by ID
POST	/students/createStudent	Add a new student
PUT	/students/updateStudent	Update student info
DELETE	/students/deleteStudent/{id}	Delete student
cURL example
curl -X GET http://localhost:8080/students/getAllStudents

⚙️ Tech Stack

Java 17 + Spring Boot

MySQL 8

Docker

Kubernetes (Minikube)

Maven

Lombok

🔐 Kubernetes Secret (Example)
apiVersion: v1
kind: Secret
metadata:
  name: mysql-secret
type: Opaque
data:
  MYSQL_ROOT_PASSWORD: cGFzc3dvcmQ=

🛠 Troubleshooting
Issue	Fix
ImagePullBackOff	Make sure image is loaded in Minikube
API not accessible	Check server.address=0.0.0.0
MySQL connection error	Ensure PVC + MySQL pod are running
📌 Author

Amina Daghari
Student at ESPRIT, Tunisia 🇹🇳
📎 PRs & Stars are welcome!
