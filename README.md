# kubernates-street


                                       Sample Architecture of Kubernates

kubernates its mainly used for container orchestration purpose.

in K8's we have pods. inside  pods we can run our docker or rocket containers.

   ![asdf](https://user-images.githubusercontent.com/38804803/60718533-86383800-9f42-11e9-825a-126e35a35ffc.jpg)

Master dont have pods.only slaves has pods.

every pod have ip address.

inside pods we can run multiple containers.

main thing is we can choose conntainer with out have port conflicts.

![kubernetes-reference-architecture](https://user-images.githubusercontent.com/38804803/60717605-e679aa80-9f3f-11e9-9e69-b0e0cb4026bf.jpg)

     Scheduler:
        it's scheduling pods accross multiple nodes.
        
     Control manager:
        node controller 
        replication controller
        end point controller
        service accountant token controller
        
    it is resposible for overall health of entire cluster.it ensures that nodes are up and running all the times correct pods are running
    mentioned in the spec file.
  
      etcd:
        its a light weighted key value database it is devoloped by core os.
    
        it is very popular key value database.
    
        it store cuurent cluster state any point of time
     
