Requirements:
- Docker
- Kubernetes
- Kong ingress controller

You will need to build the docker image of the NER engine, as the NER engine is a few GB large and my PC is unable to push it. You can find the build here: https://github.com/SC4-0/joint-ic-sf-model/tree/main/v3. Kong ingress controller can be installed via: `helm install kong/kong --generate-name --set ingressController.installCRDs=false`.

Steps:
1. Go to directory containing the Dockerfile (`cd joint-ic-sf-model/v3`)
2. Run `docker build -t ner-engine:1.0 .`
3. Replace "kxingjing/ner-engine:1.1" on line 111 of backend.yaml with ner-engine:1.0
4. Run `kubectl apply -f backend.yaml`
5. Check that back-end is ready before running front-end.

How to check that back-end is ready:
Because readiness probes have not been set up yet, there is no distinction between the container running vs. process in container is running and ready to receive requests. Hence you cannot use the READY or STATUS field to gauge readiness.
1. Check that the RabbitMQ message broker is ready by accessing localhost:32000
2. Check that the ASR engine is ready by checking its logs using kubectl logs
3. Check that NER-engine is ready by checking its logs using kubectl logs