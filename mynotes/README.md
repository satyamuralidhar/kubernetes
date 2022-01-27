User Managment:
==============

create a usercreds:
=================

kubectl create namespace devteam
mkdir devcreds ==> to use this directory to store user creds
openssl genrsa -out developer.key 2048
openssl req -new -key developer.key -out developer.csr -subj "/CN=developer/O=javadeveloper"
cn=commonname;O=organization
cat developer.csr
* sign this certificate with cluster ca
ls -tlh /etc/kubernetes/pki ==> this is the certificate location of kubernetes(kubeadm)
openssl x509 -req -in developer.csr -CA /etc/kuberntes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out developer.crt -days 500
developer.csr , developer.crt , developer.key ==> by using these keys user can access k8s env.

create a role for ns:
====================
rbac.yaml

kind: Role
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
    namespace: javaproject
    name: deployment-manager-role
rules:
- apiGroups: ["extensions","apps"]
  resources: ["deployments","repicasets","pods"]
  verbs: ["get","list","watch","create","update","patch","delete"]

kubectl create -f rbac.yaml

role-binding:
============
rolebind.yaml

kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
    namespace: javaproject
    name: deployment-manager-rolebinding
subjects:
- kind: User
  name: developer
  apiGroup: ""
roleRef:
    kind: role
    name: deployment-manager-role
    apiGroup: ""

kubectl apply -f rolebinding.yaml 

Note: set up developer user account can communicate to kubenetes cluster machine

kubectl config set-credentials developer --client-certificate=root/developer/developer.crt --client-key=/root/developer/developer.key 

kubectl config set-context developer-context --cluster=kubernetes --namespace=javaproject --user=developer

after that we need to verify config file

cat ~/.kube/config

kubectl --kubeconfig ./admin.config config view -o jsonpath='{.contexts[*].name}'
kubectl --kubeconfig ./admin.config get pods --namespace=javaproject
