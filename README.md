#Command line


gcloud services enable container.googleapis.com
gcloud services enable sqladmin.googleapis.com

#2 nodes in Toronto-a zone, 1  node in Toronto-b zone
gcloud container clusters create todo-list-cluster-pro \
    --zone=northamerica-northeast2-a \
    --num-nodes=2

gcloud container node-pools create additional-pool \
    --cluster=todo-list-cluster-pro \
    --zone=northamerica-northeast2-a \
    --node-locations=northamerica-northeast2-b \
    --num-nodes=1

gcloud container clusters get-credentials todo-list-cluster-pro --zone northamerica-northeast2-a --project ece-9016-group-02-project



gcloud auth configure-docker
docker build -t gcr.io/ece-9016-group-02-project/todo-backend .

docker push gcr.io/ece-9016-group-02-project/todo-backend


kubectl create configmap mysql-init-db-config --from-file=./database/init.sql
kubectl create configmap nginx-config --from-file=nginx.conf
kubectl create configmap nginx-html-config --from-file=./frontend/index.html --from-file=./frontend/script.js --from-file=./frontend/style.css


kubectl apply -f k8s/ 

kubectl  get pods -o wide

kubectl get services



stop:
gcloud container clusters resize todo-list-cluster-pro --zone=northamerica-northeast2-a --num-nodes=0 --node-pool=default-pool
gcloud container clusters resize todo-list-cluster-pro --zone=northamerica-northeast2-a --num-nodes=0 --node-pool=additional-pool


start:
gcloud container clusters resize todo-list-cluster-pro --zone=northamerica-northeast2-a --num-nodes=2 --node-pool=default-pool

gcloud container clusters resize todo-list-cluster-pro --zone=northamerica-northeast2-b --num-nodes=1 --node-pool=additional-pool

