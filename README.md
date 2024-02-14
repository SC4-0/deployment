This deployment repository contains the code for deploying the back-end. Requirements:
- Docker
- Kubernetes
- Kong ingress controller

Docker can be installed from https://docs.docker.com/get-docker. It will come with Kubernetes.

Kong ingress controller can be installed via: `helm install kong/kong --generate-name --set ingressController.installCRDs=false`.

If there are docker images cited in backend.yaml that are not available online, please build them from the appropriate SC4.0 repository. Below is an example for the NER engine. You can find the build here: https://github.com/SC4-0/joint-ic-sf-model/tree/main/v3.
Steps:
1. Go to directory containing the Dockerfile (`cd joint-ic-sf-model/v3`)
2. Run `docker build -t ner-engine:1.0 .`
3. Replace "kxingjing/ner-engine:1.1" on line 111 of backend.yaml with ner-engine:1.0
4. Run `kubectl apply -f backend.yaml`
5. Check that back-end is ready before running front-end.

How to deploy the back-end:
1. Go to the directory containing backend.yaml
2. Run `kubectl apply -f backend.yaml`
3. Check that the back-end has deployed with no issues with `kubectl get pods`, looking at the STATUS and READY fields.
4. Kubernetes does not make a distinction when reporting STATUS and READY, unless readiness probes are set up, between the container running vs. process in container running and ready to receive requests. In other words, to check that the processes in the container are ready, and not just that the container itself is ready, you can view the process output in the container with `kubectl logs` or by accessing localhost, e.g. localhost:32000 for the RabbitMQ message broker, localhost:30002/docs for the order planning server documentation.
5. Close the backend by running `kubectl delete -f backend.yaml` and `kubectl delete pvc hostpath-volume-mssql-0`
