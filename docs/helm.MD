#https://devopscube.com/install-configure-helm-kubernetes/


installation of helm: 

     curl -L https://git.io/get_helm.sh | bash
     
Create Tiller Service Account With Cluster Admin Permissions

Tiller is the server component for helm. Tiller will be present in the kubernetes cluster and the helm client talks to it for deploying applications using helm charts.

Helm will be managing your cluster resources. So we need to add necessary permissions to the tiller components which resides in the cluster kube-system namespace.

Here is what we will do,

*Create a service account names tiller

*Create a ClusterRoleBinding with cluster-admin permissions to the tiller service account.

      #kubectl apply -f helm-rbac.yaml
      
 Initialize Helm: Deploy Tiller
Next step is to initialize helm. When you initialize helm, a deployment named tiller-deploy will be deployed in the kube-system namespace.
Initialize helm using the following command.

                    #helm init --service-account=tiller --history-max 300

If you want a specific tiller version to be installed, you can specify the tiller image link in the init command using --tiller-image flag. You can find the all tiller docker images in public google GCR registry.

                 #helm init --service-account=tiller --tiller-image=gcr.io/kubernetes-helm/tiller:v2.14.1   --history-max 300
                 
 
        
If you dont mention “–service-account=tiller”, you will get the following error.

1
Error: no available release name found
You can check the tiller deployment in the kube-system namespace using kubectl.


                         #kubectl get deployment tiller-deploy -n kube-system

Deploy a Sample App Using Helm

Now lets deploy a sample nginx ingress using helm.

Execute the following helm install command to deploy an nginx ingress in the kubernetes cluster. It will download the nginx-ingress helm chart from the public github helm chart repo.


                      #helm install stable/nginx-ingress --name nginx-ingress

You can check the install helm chart using the following command.

                        #helm ls

You can delete the sample deployment using delete command. For example,


                       #helm delete nginx-ingress

Remove Helm (Tiller) From Kubernetes Cluster

If you want to remove the tiller installtion from the kubernetes cluster use the following command.

                        #helm reset

For some reason, if it throws error, force remove it using the following command.


                        #helm reset --force

Also you can use the kubectl command to remove it.


                       #kubectl delete deployment tiller-deploy --namespace kube-system


In the next blog post, we will look in to chart development and best practices of HELM

