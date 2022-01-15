docker exec -it flask-hello_flask_1 bash

docker exec -it flask-hello_flask_1 python train_model.py

docker-compose up --build


curl --header "Content-Type: application/json" \
  --request POST \
  --data '{"flower":"1,2,3,4"}' \
  http://localhost:5000/iris_post


chmod 400 flask.pem

ssh -i "flask.pem" ec2-user@compute.amazonaws.com


sudo yum install -y docker git
sudo chkconfig docker on
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose version
docker ps