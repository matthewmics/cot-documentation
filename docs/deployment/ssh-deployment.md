# Deploy via SSH

## Overview

In this section, we'll document the process of deploying the application to a remote server via SSH. We'll be using AWS EC2 instances for hosting, and you will need an AWS PEM file to authenticate securely. By following these steps, you'll be able to upload and run both the API and client on the server for production purposes. The PEM file will ensure secure access to the EC2 instance, and we will deploy the code using Git and necessary deployment scripts.

## Deployment Process via SSH
```
ssh ubuntu@3.1.208.97 -i cxforms-keypair.pem
```

Once inside the server, the production and test directories for the application are as follows:

* Production directory: /home/ubuntu/recruitment-system

* Test directory: /home/ubuntu/test/recruitment-system

To deploy the latest version of the application, follow these steps:

1. Navigate to the respective directory (either `recruitment-system` for production or `test/recruitment-system` for the test environment):

```
cd /home/ubuntu/recruitment-system
```
or
```
cd /home/ubuntu/test/recruitment-system
```

2. Pull the latest changes from Git:
```
git pull
```

## Deploying the API

1. Navigate to the api directory:
```
cd api
```

2. Build the API:
```
npm run build
```

3. Run the migrations:
```
npm run orm:run
```

4. Reload the API with PM2:

* For production:
```
pm2 reload api
```
* For test environment:
```
pm2 reload apiex
```

## Deploying the Client

1. Navigate to the client directory:
```
cd /client
```

2. Build the client:
```
npm run build
```

This will deploy the client-side application and ensure the latest version of the API and client are live.

## Conclusion

Deploying the application via SSH involves pulling the latest code from the repository, building the API and client, running the necessary database migrations, and reloading the services using PM2. By following the steps outlined above, you ensure that both the production and test environments are kept up-to-date with the latest changes.

Always make sure to run the required build and migration commands before reloading the services to ensure smooth deployment and operation of the application. If you encounter any issues, reviewing the logs and ensuring the correct configurations are set for both the API and client will help resolve them efficiently.