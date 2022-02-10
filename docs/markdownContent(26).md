## What is MongoDB Atlas?

![](/images/mongo-atlas-0.png)

> MongoDB Atlas is a global cloud document database service for modern applications. Deploying a fully managed MongoDB helps to ensure availability, scalability, and security compliance by using intelligent automation to maintain performance at scale as your applications evolve.

### Create Account On MongoDB Atlas
1. Go to the MongoDB Atlas [landing page](https://cloud.mongodb.com).

2. Fill in the required information (email address, first name, last name, and password).

3. Click the terms of service and privacy policy links, which should open on a new tab.
If you want to continue with the registration, select the I agree to the terms of service and privacy policy check box.

5. Click the Get started free at the bottom of the form.

6. The website will ask to choose a cluster. Choose Starter Clusters and click on Create a cluster.

![](/images/mongodb-1.png)


You should also receive a welcome email, which confirms that you’re registered with MongoDB Atlas and includes a link for logging onto the service.

1. In the Cloud Provider & Region section, the AWS option should be selected as the default provider, but you can select any provider. All three platforms support the free tier.

2. Beneath the list of providers, select a region.

3. Expand the Cluster Tier section and ensure that M0 Sandbox is selected. This is the free M0 service level.

4.  Expand the Cluster Name section and type Cluster1 in the text box.

![](/images/mongo-atlas-2.png)

1.  Click the Create Cluster button at the bottom of the web page.

2.  You should then receive a message stating that your cluster is being created.
When the process is complete, you’ll be taken to the Clusters page, which includes a listing for your new cluster, as shown in the following figure.

![](/images/mongo-atlas-3.png)

The cluster information includes the service level, cloud provider, and region, along with details about operations, connections, and logical size, all of which currently show zero amounts.


### Configure IP address and connection string

Next, you’ll configure MongoDB Atlas to connect to the cluster you created.

**For this, you will need the IP address of the device that will connect to the service.**

If you plan to connect to MongoDB Atlas on the same device where you’re setting up the service, MongoDB Atlas can find the local IP address automatically.

As part of this exercise, you’ll also set up an administrator account for accessing the cluster.

1. In the Cluster1 section of the Clusters page, click the CONNECT button in the left pane.

![](/images/mongo-atlas-4.png)

The Connect to Cluster1 dialog box appears, showing the two steps you must take to configure your connection.

![](/images/mongo-atlas-5.png)

1. For security reasons, MongoDB Atlas blocks all outside connections by default. In order to connect, you must first whitelist your IP address. In the Step 1 section, do one of the following:
![](/images/mongo-atlas-6.png)

* If you plan to connect from the computer you’re currently using, click Add Your Current IP Address and then click Add IP Address.
* If you plan to connect from a different computer, click Add a Different IP Address, type the IP address, and then click Add IP Address. You can edit the IP address, add IP addresses, or delete them at any time in case you change devices.

1. In the Step 2 section, type admin in the Username text box (or whatever name you want to use), and then type a password in the Password text box.

To make it easier to connect to MongoDB Atlas from Studio 3T, your password should include only alphanumeric characters, that is, letters and numbers only with no special characters.

If you use special characters, you will need to encode them when creating a connection string for accessing the MongoDB service.

> To generate a password automatically, click the Autogenerate Secure Password button. A password will be generated that includes only alphanumeric characters. Be sure to save the password somewhere secure.

4. choice the connect to your application and take the URL like this
`mongodb+srv://<USERNAME>:<PASSWORD>@cluster0.vheog.mongodb.net/<DATABASE_NAME>

5. take this URL and set it at the Heroku env vars