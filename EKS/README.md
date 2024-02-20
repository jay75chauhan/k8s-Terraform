![image](https://github.com/openupthecloud/k8s-starter-eks-terraform/assets/5528307/9df77164-c4b2-4107-8802-bb06871ae4c1)

## Getting Started

> Requires at least 1 t2.medium instance to run the required containers and the workload.

### Step 1: Install dependencies (AWS, Terraform, Kubectl)

```sh
./install.sh
```

### Step 2: Deploy the infrastructure with Terraform

Update VPC default in `./variables.tf`

```sh
terraform apply
```

Or pass it as the vpc ID variable:

```sh
terraform apply -var vpc_id=$(aws ec2 describe-vpcs | jq -r .Vpcs[0].VpcId)
```

### Step 3: Connect KubeCTL to the deployed cluster

```sh
aws eks update-kubeconfig --region us-east-1 --name my-cluster-eks
```

### Step 4: Create an app with a deployment and service

```sh
kubectl apply -f app.yaml
```

#### How can I see what is running in my cluster when deployed?

`kubectl get pods -n eks-sample-app`
`kubectl get all -n eks-sample-app`

#### How can I debug my deployed service?

You can access your running pod or service by running:

`kubectl exec -it <deployment-id> -n eks-sample-app -- /bin/bash`

For example:

`kubectl exec -it eks-sample-linux-deployment-5b568bf897-5xkf7 -n eks-sample-app -- /bin/bash`

#### How can I visualise what is deployed in the terraform module?

Running `terraform state show` will show you the list of resources inside your module once deployed. Using `terraform graph` you can also export your graph to a tool like: https://edotor.net/ to visualise the infra. However, be careful not to share any sensitive information contained in your state file(s) with external or 3rd parties.
