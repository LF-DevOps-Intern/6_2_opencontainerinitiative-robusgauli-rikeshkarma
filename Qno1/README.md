# Deploy Postgres database using PVC & PV cluster.

Firstly we need to Dockerize PostgreSQL, for this Kubernetes can pull Docker images from a registry and deploy them based on the configuration. We can create our own Docker image or we can use the official image from Docker Hub. For this task we will be using the latest postgres image.

Now let’s start by creating our Connection Configuration and Secretes:
- We need to store some connection configuration for the PostgreSQL instance using the Kubernetes secrets config. This will make sure that the sensitive information (say database credentials) are not stored in plain text.
- This provider stores the secretes as base64 strings by default, so let’s create the configuration for the secretes. Let’s say the password to our database is “password123”, then let’s convert our password to base64 strings format using the following command:
  - `echo “password123” | base64`<br/>
  ![convert password to base64](https://github.com/LF-DevOps-Intern/6_2_opencontainerinitiative-robusgauli-rikeshkarma/blob/master/Qno1/snapshots/convert%20password%20to%20base64.png)
- After this step, let’s create a secret config file named “postgres-secretes.yml” and apply it to the cluster. We need to add these lines as in snapshot below:
  ![postgres secretes yml](https://github.com/LF-DevOps-Intern/6_2_opencontainerinitiative-robusgauli-rikeshkarma/blob/master/Qno1/snapshots/postgres-secretes%20yml.png)
- Now we can apply this config and then verify that the contents are stored correctly using the following commands:
  - `kubectl apply -f postgres-secretes.yml`<br/>
  ![apply postgres secretes](https://github.com/LF-DevOps-Intern/6_2_opencontainerinitiative-robusgauli-rikeshkarma/blob/master/Qno1/snapshots/apply%20postgres%20secretes.png)
  - `kubectl get secret postgres-secret-config -o yaml`<br/>
  ![get secrete](https://github.com/LF-DevOps-Intern/6_2_opencontainerinitiative-robusgauli-rikeshkarma/blob/master/Qno1/snapshots/get%20secrete.png)
Now let’s create a PersistentVolumeClaim (PVC) and PersistentVolume (PV) cluster.
- We have to create a permanent file storage for database data because the Docker instance doesn’t persist any information when the container no longer exists (by default). And the solution to this is to mount a filesystem to store the data. Kubernetes has different configuration formats for those operations.
- First, we need to create a PersistentVolume manifest that describes the type of volumes we want to use. We define the configuration for the PersistentVolume as in snapshot below:<br/>
  ![config for PV](https://github.com/LF-DevOps-Intern/6_2_opencontainerinitiative-robusgauli-rikeshkarma/blob/master/Qno1/snapshots/config%20for%20PV.png)
  - In the above configuration, we have instructed to reserve 5GB of read-write storage at _/mnt/data_ on the cluster’s node.
  - Now, we can apply it and check that the persistent volume is available using the following command:
    - `kubectl apply -f pv-volume.yml`<br/>
  ![apply pv-volume](https://github.com/LF-DevOps-Intern/6_2_opencontainerinitiative-robusgauli-rikeshkarma/blob/master/Qno1/snapshots/apply%20pv-volume.png)
    - `kubectl get pv postgres-pv-volume`<br/>
  ![checking PV availability](https://github.com/LF-DevOps-Intern/6_2_opencontainerinitiative-robusgauli-rikeshkarma/blob/master/Qno1/snapshots/checking%20PV%20avaibality.png)
- Then, after this we create a PersistentVolumeClaim that requests the usage for that particular PersistentVolume type based on the same storage class. Also importantly we need to follow up with a PersistentVolumeClaim configuration that matches the details of the previous manifest. We define the configuration of the PersistentVolumeClaim as in snapshot below:<br/>
  ![config PV-claim](https://github.com/LF-DevOps-Intern/6_2_opencontainerinitiative-robusgauli-rikeshkarma/blob/master/Qno1/snapshots/config%20PV-claim.png)
  - In the above configuration , we have requested a PersistentVolumeClaim for 1GB of data using the same storage class name. This is an important parameter because it enables Kubernetes to reserve 1GB of the available 5GB of the same storage class for this claim.
  - Now we can apply it and check that the persistent volume claim is bounded using the following commands:
    - `kubectl apply -f pv-claim.yml`<br/>
  ![apply pv-vlaim](https://github.com/LF-DevOps-Intern/6_2_opencontainerinitiative-robusgauli-rikeshkarma/blob/master/Qno1/snapshots/apply%20pv-claim.png)
    - `kubectl get pvc postgres-pv-claim`<br/>
  ![check pv-claim](https://github.com/LF-DevOps-Intern/6_2_opencontainerinitiative-robusgauli-rikeshkarma/blob/master/Qno1/snapshots/check%20pv-claim.png)
Next, it’s now ready for deployment. We need to issue a deployment config for our instance that uses the settings from the post-secret-config secret name. We also need to reference PersistentVolume and PersistentVolumeClaim that we created earlier. We define the configuration of the deployment as in the snapshot below:<br/>
  ![postgres-deployment](https://github.com/LF-DevOps-Intern/6_2_opencontainerinitiative-robusgauli-rikeshkarma/blob/master/Qno1/snapshots/postgres-deployment.png)
- We can see that we have combined all the configurations together that we defined earlier with the secret config and the persistent volume mounts. We used the apiVersion: apps/v1 deployment config, which requires us to specify a few lines, such as selector and metadata fields. Then, we added details of the container image and the image pull policy. This is all very important to ensure that we have the right volume and secretes used for that container.
- Now, finally we can apply the deployment and check that it is up and available using the following commands:
  - `kubectl apply -f postgres-deployment.yml`<br/>
  ![apply postgres-deployment](https://github.com/LF-DevOps-Intern/6_2_opencontainerinitiative-robusgauli-rikeshkarma/blob/master/Qno1/snapshots/apply%20postgres-deployment.png)
  - `kubectl get deployments`<br/>
  ![checking postgres deployment](https://github.com/LF-DevOps-Intern/6_2_opencontainerinitiative-robusgauli-rikeshkarma/blob/master/Qno1/snapshots/checking%20postgres%20deployment.png)
  - We can use the following to check if the PVC is connected to the PV successfully:
    - `kubectl get pvc`<br/>
  ![kubectl get pvc](https://github.com/LF-DevOps-Intern/6_2_opencontainerinitiative-robusgauli-rikeshkarma/blob/master/Qno1/snapshots/kubectl%20get%20pvc.png)
    - The status column shows that the claim is Bound, which means they are connected.
  
We can see that there is a postgres deployment in the list and it's ready and very much healthy.