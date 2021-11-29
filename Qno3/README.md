# Connect Postgres database from Postgres Client using core-dns's host name.

Above we have connected to the Postgres database internally using Kubectl, and now let’s connect to our Postgres database from POstgres Client using core-dns’s host name.

Firstly let’s export the POSTGRES_PASSWORD environment variable to be able to connect/ log into the PostgreSQL instance because we have stored the password in the secretes as base64 strings: Let’s use the following command:
    - `export POSTGRES_PASSWORD=$(kubectl get secret postgres-secret-config -o jsonpath="{.data.password}" | base64 --decode)`<br/>
  ![export postgres password](https://github.com/LF-DevOps-Intern/6_1_k8s-roubusgauli-rikeshkarma/blob/master/Assignment%202/Qno3/snapshots/export%20postgres%20password.png)
    - This will store our base64 password in the POSTGRES_PASSWORD.
- Now we can open another terminal and type the following command to forward the Postgres port:
  - `kubectl port-forward svc/postgres-secret-config 5432:5432`<br/>
  ![port forwarding](https://github.com/LF-DevOps-Intern/6_1_k8s-roubusgauli-rikeshkarma/blob/master/Assignment%202/Qno3/snapshots/port%20forwarding.png)
  - After this the system will start handling the port connection.
- We can now minimize the port-forwarding window and return to the previous one and type the following command to connect to psql (a postgreSQL client):
  - `PGPASSWORD="$POSTGRES_PASSWORD" psql --host 127.0.0.1 -U postgres -d postgres -p 5432`<br/>
  ![connect through psql](https://github.com/LF-DevOps-Intern/6_1_k8s-roubusgauli-rikeshkarma/blob/master/Assignment%202/Qno3/snapshots/connecting%20througl%20psql.png)
- After this we observe that the psql command prompt appears, and PostgreSQL is ready to receive our input.