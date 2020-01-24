# Brief Setup for your GCP Kubernetes gateway w/SSL Key defined
Repo to enable GCP Kubernetes Engine cluster with Layer7 API Gateway

## Create kubernete clusters, sample commands are:
` gcloud container clusters create cicd --no-enable-autorepair --num-nodes=1 --machine-type=n1-standard-4 `

` gcloud container clusters get-credentials cicd `

## Create kubernetes services using the SSG yaml file:
` kubectl apply -f ssl_k8s/mysql-pv-claim.yml `

` kubectl apply -f ssl_k8s/l7-gateway-config-ssl.yml -f ssl_k8s/l7-gateway-mysql.yml -f ssl_k8s/l7-gateway-ssl.yml `

## Check services are up
` kubectl get deployments `

` kubectl get pods `

` kubectl get services `

` kubectl describe pods gw-k8s `

## Delete the kubernetes services:
` kubectl delete -f ssl_k8s/l7-gateway-config-ssl.yml -f ssl_k8s/l7-gateway-mysql.yml -f ssl_k8s/l7-gateway-ssl.yml `

` kubectl get pods `

` kubectl delete pod gw-k8s-76bd8669cd-fwqj7  `

` kubectl delete deployment gw-k8s `

## Delete cluster:
` gcloud container clusters delete cicd `


## <a name="development"></a>Development:

### Custom certificates:

Create your own x509 server certificates and keys for the gateway. This requires **openssl** to be installed on your local system.

Create the self-signed certificate for the gateway using 'gw1.aricday.net' as the subject and SAN

 `   openssl req -new -x509 -days 730 -nodes -newkey rsa:4096 -keyout config/certs/mas.key -subj "/CN=gw1.aricday.net" -config <(sed 's/\[ v3_ca \]/\[ v3_ca \]\'$'\nsubjectAltName=DNS:gw1.aricday.net/' /etc/ssl/openssl.cnf) -out config/certs/k8s.cert.pem    `

Convert the PEM certificate to PKCS12:

  `   openssl pkcs12 -export -clcerts -in config/certs/k8s.cert.pem -inkey config/certs/k8s.key -out config/certs/k8s.cert.p12  `

### Edit the values to suit your needs, then run something like for variable input:
` echo "LICENSE=\"$(gzip -c add-ons/license/license.xml | base64 --wrap=0)\"" > add-ons/license/license.gz.base64 `

` echo "SSLKEY=\"$(cat config/certs/k8s.cert.p12 | base64)\"" > config/certs/k8s.cert.p12.base64 `
