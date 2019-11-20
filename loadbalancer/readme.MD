deploy pod
      
      #kubectl create -f deploy.yml
      
      
      to see the deployment 
      
      
      #kubectl get deploy 

expose the pod 

       #kubectl expose deploy tomcat-deploy --name=tomcat-service --port=80 --target-port=8080 --type=NodePort
       
       #kubectl get svc tomcat-service
       output:  tomcat-service   NodePort    10.96.222.246   <none>        80:30672/TCP     57m
       
       then http://nodeip:30672
        then we will get tomcat dashboard

