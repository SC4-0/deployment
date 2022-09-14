Due to the rest of the services in the cluster needing to connect to the rabbitmq message broker when starting, the message broker has been separated out for the user to run prior to running the rest of the services.

Steps:
1. kubectl apply -f rabbit.yaml
2. Wait for the rabbitmq message broker to go live; you can check on port 32000
3. kubectl apply -f backend.yaml
4. Wait for ner engine and vosk engine to go live before running front-end