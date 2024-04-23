# CKA-Practice-Exam-Lab-


1. 


CKA-beta
Lab launched. It will end if the time runs out or you press the "Finish" button in the last task.
0%
completed
2:00:17left
Challenge

Show hint

Watch video

Context:

You have been asked to create a new ClusterRole for a robotics pipeline and bind it to a specific ServiceAccount scoped to a specific namespace.

Task:

Create a new ClusterRole named robotics-clusterrole, which only allows to create the following resource types:

✑ Deployment

✑ StatefulSet

✑ DaemonSet

Create a new ServiceAccount named robo-token in the existing namespace machine-team1. Bind the new ClusterRole robotics-clusterrole to the new ServiceAccount robo-token, limited to the namespace machine-team1. ClusterRoleBinding name should be robotics-cb.




--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

vi clusterrole.yaml
vi serviceaccount.yaml
vi clusterrolebinding.yaml


kubectl apply -f clusterrole.yaml
kubectl apply -f serviceaccount.yaml
kubectl apply -f clusterrolebinding.yaml





--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


2. 



Challenge


You can get the node name by running this command: kubectl get nodes -l=LABEL_KEY=LABEL_VALUE

Task:

Set the node with label team=fixers as unavailable and reschedule all the pods running on it.




--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


kubectl get nodes -l team=fixers
 
kubectl drain ip-172-31-19-158.us-east-2.compute.internal --ignore-daemonsets --delete-emptydir-data --force

!!!!!or use script !!! : 

echo -e '#!/bin/bash\n\n# Label selector to identify the nodes\nLABEL_SELECTOR="team=fixers"\n\n# Get the list of node names based on the label selector\nNODES=$(kubectl get nodes -l $LABEL_SELECTOR -o name)\n\n# Loop through the list of nodes and drain them\nfor NODE in $NODES; do\n    echo "Draining $NODE"\n    kubectl drain $NODE --ignore-daemonsets --delete-emptydir-data --force\ndone' > drain_nodes.sh; chmod +x drain_nodes.sh; ./drain_nodes.sh





 
kubectl delete pod cloud-app --namespace default
 
kubectl delete pod devflow --namespace default
 
kubectl get pods --all-namespaces -o wide

kubectl get nodes
kubectl get pods --all-namespaces -o wide

kubectl get events --sort-by='.metadata.creationTimestamp'






--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

3. 


Task:

Create a new nginx Ingress resource as follows:

✑ Name: hello-ingress

✑ Namespace: ingress-deploy

✑ Exposing service hello on path /hello using service port 80




--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

kubectl create namespace ingress-deploy

vi hello-ingress.yaml

kubectl apply -f hello-ingress.yaml


kubectl get ingress hello-ingress -n ingress-deploy
kubectl describe ingress hello-ingress -n ingress-deploy



--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


4. 


Task:

Create a new PersistentVolumeClaim:

✑ Name: pvс-fs

✑ Class: csi-hostpath-sc

✑ Capacity: 10Mi

Create a new Pod which mounts the PersistentVolumeClaim as a volume:

✑ Name: devops-server

✑ Image: nginx

✑ Mount path: /usr/share/nginx/html

Configure the new Pod to have ReadWriteOnce access on the volume.

Info:

The pod and pvc might be in a pending state and it is okay. Just make sure to create these resources.



--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

vi pvc-fs.yaml
vi devops-server-pod-pvc.yaml
kubectl apply -f pvc-fs.yaml 
kubectl apply -f devops-server-pod-pvc.yaml 

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


5. 


Task:

Create a new NetworkPolicy named allow-from-ns-port in the existing namespace dev1. Ensure that the new NetworkPolicy allows Pods in namespace dev2 to connect to port 9000 of Pods in namespace dev1. Further ensure that the new NetworkPolicy:

✑ does not allow access to Pods, which don't listen on port 9000

✑ does not allow access from Pods, which are not in namespace dev2.

Info:

You can use label of the namespace 'dev2' to specify in the ingress. Label is 'purpose=production'.



----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

vi allow-from-ns-port.yaml
kubectl apply -f allow-from-ns-port.yaml
kubectl get networkpolicy -n dev1
kubectl describe networkpolicy allow-from-ns-port -n dev1

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


6. 


Task:

Reconfigure the existing deployment web and add a port specification named http exposing port 80/tcp of the existing container nginx.

Create a new service named web-svc exposing the container port http.

Configure the new service to also expose the individual Pods via a NodePort on the nodes on which they are scheduled.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

vi web-dep.yaml
kubectl apply -f web-dep.yaml
vi web-svc.yaml
kubectl apply -f web-svc.yaml
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

7. 


Task:

Scale the deployment cookie-app to 3 pods.



----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

kubectl scale deployment cookie-app --replicas=3
kubectl get deployment cookie-app


----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

8. 



Task:

Schedule a pod as follows:

✑ Name: processor

✑ Image: nginx

✑ Node selector: gpu=true


----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 vi processor-pod.yaml
 kubectl apply -f processor-pod.yaml
 kubectl get pods
 kubectl describe pod processor

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
9.

Task:

Check to see how many nodes are ready (not including nodes tainted NoSchedule) and write the number to /home/ec2-user/cka/ready_nodes


----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------




----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



10.


Task:

Schedule a Pod as follows:

✑ Name: quantum

✑ App Containers: 2

✑ Container Name/Images:

- nginx

- redis






----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

vi quantum-pod.yaml
kubectl apply -f quantum-pod.yaml
kubectl get pods quantum
kubectl describe pod quantum


----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

11.

Task:

Create a persistent volume with name crypto-data, of capacity 2Gi and access mode ReadOnlyMany. The type of volume is hostPath and its location is /srv/crypto-data


----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
vi crypto-data-pv.yaml
kubectl apply -f crypto-data-pv.yaml

kubectl get pv crypto-data
kubectl describe pv crypto-data

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


12.
Task:

Monitor the logs of pod devflow and:

✑ Extract log lines corresponding to error file-not-found

✑ Write them to /home/ec2-user/cka/logs-12

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

kubectl logs devflow | grep "file-not-found" > /home/ec2-user/cka/logs-12
cat /home/ec2-user/cka/logs-12


----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

13.

CKA-beta

Context:

An existing Pod needs to be integrated into the Kubernetes built-in logging architecture (e.g. kubectl logs). Adding a streaming sidecar container is a good and common way to accomplish this requirement.

Task:

Add a sidecar container named helper, using the busybox image, to the existing Pod cloud-app. The new sidecar container has to run the following:

command: `["/bin/sh", "-c", "tail -n+1 -f /var/log/cloud-app.log "]`
Use a Volume, mounted at /var/log, to make the log file big-corp-app.log available to the sidecar container.

Info:

Don't modify the specification of the existing container other than adding the required volume mount. 

You can delete and recreate the pod if it gives error when updating.


----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

vi cloud-app-pod.yaml

kubectl apply -f cloud-app-pod.yaml
kubectl get pods
kubectl logs cloud-app -c helper

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


14.

Task:

From the pod label name=overloaded-cpu, find pods running high CPU workloads and write the name of the pod consuming most CPU to the file /home/ec2-user/cka/overloaded-cpu-name.txt (which already exists).

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
kubectl top pod -l name=overloaded-cpu --sort-by=cpu | head -2 | tail -1 | awk '{print $1}' > /home/ec2-user/cka/overloaded-cpu-name.txt
cat /home/ec2-user/cka/overloaded-cpu-name.txt
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

15.

Task:

A Kubernetes worker node, with label state=not-ready is in state NotReady. Investigate why this is the case, and perform any appropriate steps to bring the node to a Ready state, ensuring that any changes are made permanent.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
kubectl get nodes -l state=not-ready
sudo ssh -i ~/.ssh/id_rsa ec2-user@ip-172-31-17-189.us-east-2.compute.internal

inside ssh yes 

sudo systemctl status kubelet
sudo systemctl start kubelet
sudo systemctl enable kubelet
sudo systemctl status kubelet

exit


kubectl get nodes



OR 


echo -e '#!/bin/bash\n\n# Fetch the name of the node that is not ready\nNODE=$(kubectl get nodes -l state=not-ready -o jsonpath="{.items[0].metadata.name}")\necho "Node identified for maintenance: $NODE"\n\n# Check if the node name is empty\nif [ -z "$NODE" ]; then\n    echo "No NotReady node found with the specified label."\n    exit 1\nfi\n\n# SSH command sequence\nCOMMANDS="sudo systemctl status kubelet; sudo systemctl start kubelet; sudo systemctl enable kubelet; sudo systemctl status kubelet"\necho "Executing commands on the node via SSH"\n\n# Execute SSH command\nsudo ssh -i ~/.ssh/id_rsa ec2-user@$NODE "$COMMANDS"\n\n# Output the status of all nodes\nkubectl get nodes' > fix_node.sh && chmod +x fix_node.sh && ./fix_node.sh



----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


16.




----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
sudo mkdir -p /var/lib/backups/


export ETCDCTL_API=3

sudo /usr/local/bin/etcdctl snapshot save /var/lib/backups/etcd-snapshot.db \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/apiserver-etcd-client.crt \
  --key=/etc/kubernetes/pki/apiserver-etcd-client.key


----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

17.

Task:

Given an existing Kubernetes cluster running version 1.20.2, upgrade all of the Kubernetes control plane and node components on the master node only to version 1.20.4.

Be sure to drain the master node before upgrading it and uncordon it after the upgrade.

You are also expected to upgrade kubelet and kubectl on the master node.

Reminder:

Do not upgrade the worker nodes, etcd, the container manager, the CNI plugin, the DNS service or any other addons.

Info:

This task doesn't work as expected. For now, skip this task.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------












































