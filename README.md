# Enable APIs
gcloud services enable container.googleapis.com
gcloud services enable sqladmin.googleapis.com


gcloud container clusters create todo-list-cluster --num-nodes=2 --zone=us-central1-a
gcloud container clusters get-credentials todo-list-cluster --zone=us-central1-a

gcloud auth configure-docker
docker build -t gcr.io/ece-9016-group-02-project/todo-backend .

docker push gcr.io/ece-9016-group-02-project/todo-backend


kubectl create configmap mysql-init-db-config --from-file=./database/init.sql
kubectl create configmap nginx-config --from-file=nginx.conf
kubectl create configmap nginx-html-config --from-file=./frontend/index.html --from-file=./frontend/script.js --from-file=./frontend/style.css


kubectl apply -f k8s/ 



