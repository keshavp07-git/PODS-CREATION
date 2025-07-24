# Pod Creation Steps with kOps

## 1. Prerequisites

- **One Node (Bastion/Management Node):** Prepare a Linux instance (e.g., Ubuntu EC2).
- **Install Required Tools:**
    - **kOps**
    - **kubectl**
    - **AWS CLI**
    - **SSH Keygen**

---

## 2. Install kOps

```sh
curl -Lo kops https://github.com/kubernetes/kops/releases/latest/download/kops-linux-amd64
chmod +x kops
sudo mv kops /usr/local/bin/
```

---

## 3. Install kubectl

```sh
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

---

## 4. Install AWS CLI

```sh
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

---

## 5. AWS CLI Login

```sh
aws configure
```
- Enter your AWS Access Key, Secret Key, region, and output format.

---

## 6. Generate SSH Key

```sh
ssh-keygen 
```
- Use the public key (`~/.ssh/id_rsa.pub`) for kOps cluster creation.

---

## 7. Next Steps

- When you will try to create pod file using command:
```
sh kubectl create -f pod.yaml
```
- Face an error like: Localhost: Unable to connect to the server: dial tcp
- **Solution:** Ensure you have created a kOps cluster and configured your `kubectl` context to point to it.

```sh
export KOPS_STATE_STORE=s3://kopskabucket
kops export kubecfg --name vpro.cloud-ops.shop
```
- It may ask username so it can be find using the command:

```sh
cat ~/.kube/config
```
Then you can use the username to login.

- Alternatively, you can set the context directly using:

```sh
kops export kubecfg --admin --name vpro.cloud-ops.shop
```
Note: Replace `vpro.cloud-ops.shop` with your actual cluster name.

-DONE !!!

## 8. Verify POd Creation
```sh
kubectl create -f pod.yaml
```
- This command will create the pod defined in your `pod.yaml` file.

```sh
kubectl get pods
``` 
- This command will list all the pods in your cluster, confirming that your pod has been created successfully.

