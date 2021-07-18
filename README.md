# Openfaas-Workshop

- Make sure that you have install docker,kubernates and local cluster minikube install on your machine
Check Docker & Kubernates version
```bash
sudo docker version
```
```bash
minikube start
kubectl version
```

#### 1. first check docker service
```bash
sudo service docker status
```
if service not started

```bash 
sudo service docker start
```
afer that login in to docker hub
```bash
sudo docker login
```

#### 2.check services on local cluster

```bash
kubectl get svc
```
#### 3. Install faas-cli

 MacOS and Linux users
```bash 
curl -sL https://cli.openfaas.com | sudo sh
```

Windows users with (Git Bash)
```bash
 curl -sL https://cli.openfaas.com | sh
```

#### 4.Install the OpenFaaS chart using arkade (recommend)

For MacOS / Linux:
```bash
curl -SLsf https://dl.get-arkade.dev/ | sudo sh
```

For Windows (using Git Bash)
```bash
curl -SLsf https://dl.get-arkade.dev/ | sh
```
#### 5.Local Kubernetes cluster or a VM
```bash
arkade install openfaas
```
Check openfaas services 
```bash
kubectl get svc -n openfaas
```

Check gateway
```bash
kubectl rollout status -n openfaas deploy/gateway
```
Port forwarding (seprete terminal)
```bash
kubectl port-forward -n openfaas svc/gateway 8080:8080
```
 when we installed openfaas that time basic-auth is also there and it is in encrypted form so we want to decrypted it
```bash
PASSWORD=$(kubectl get secret -n openfaas basic-auth -o jsonpath="{.data.basic-auth-password}" | base64 --decode; echo)
echo -n $PASSWORD | faas-cli login --username admin --password-stdin
```

```bash
echo $PASSWORD
```
so we can able to login to openfaas ui --> localhost:8080



### - Deploy first function on openfaas

#### 1.Make a directory
```bash
mkdir openfaas_demo && cd openfaas-demo
```

#### 2.Download template
You should choose which template you wantlike python .it is just pogo 
```bash
faas-cli template store pull python3
```
#### 3. Make function
```bash
faas-cli new hello-user --lang python3
```

#### 4.Open directory on editor 
```bash
code .
```

now we can make change in handler 

#### 5. Built the function
```bash
faas-cli build -f hello-user.yml
```

after successfuly build we can check build function
```bash
sudo docker images
```
#### 6. Push function into docker hub
```bash
faas-cli push -f hello-user.yml
```
#### 7. To deploy function on Openfaas
```bash
faas-cli deploy -f hello-user.yml
```

#### 8.Check built function 
```bash
faas-cli list
```
#### 9. Invoke function
```bash
echo "shivam" | faas-cli inoke hello-user
```

Thanks.






