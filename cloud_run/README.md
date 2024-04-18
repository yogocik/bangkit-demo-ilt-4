# Cloud Code Sample Repositories
This repository demonstrates how you can create a Sample Repository of code samples that you can use as a template while creating a New Application via [Cloud Code Visual Studio](https://cloud.google.com/code) plugin. Official instructions are provided [here](https://cloud.google.com/code/docs/vscode/set-up-sample-repo).

## Custom Samples Configuration File (.cctemplate)
The key thing to understand is the presence of the `.cctemplate` file in this repo. It contains the `templates` element that has entry for the folder or sample(s) that you have in this repository:
- `python-flask-api`

```
   "templates":[
       {
         "path":"python-flask-api",
         "name":"Python Flask API",
         "description":"Sample REST API built using Python Flask"
      }
   ]
}
```

## What is the same application about?
The application is a simple Inventory API application that exposes a REST API Endpoint with a couple of operations to list the inventory items and getting specific item inventory count. 

Once we deploy the API and assuming that it is hosted at `https://<somehost>`, we can access the API endpoints as follows:
- `https://<somehost>/inventory`
   This will list down all the product items with the on-hand inventory levels. 

- `https://<somehost>/inventory/{productid}`
   This will provide a single record with the productid and on-hand inventory level for that product.

The response data returned is in JSON format.

## Sample Data and API Request/Response

The application is not powered by a database at the backend to keep things simple. It contains 3 sample product ids and their on-hand inventory levels. 

| Product Id  | On-Hand Inventory Level |
| ----------- | ----------------------- |
| I-1         | 10                      | 
| I-2         | 20                      |
| I-3         | 30                      |

Sample API Request and Response are shown below:

| API Request                          | API Response                                 |
| ------------------------------------ | -------------------------------------------- |
| `https://<somehost>/inventory`       | `[{"I-1": 10, "I-2": 20, "I-3": 30}]`        |
| `https://<somehost>/inventory/I-1`   | `{"I-1": 10}`                                |
| `https://<somehost>/inventory/I-2`   | `{"I-2": 20}`                                |
| `https://<somehost>/inventory/I-200` | `{"productid": "I-200","qty": -1}`           | 


## Using the Custom Samples via Cloud Code plugin

1. Assuming that you have Visual Studio Code and the Cloud Code plugin setup, click on the Cloud Code link in the status bar.
2. Click on `New Application`
3. Select `Custom Application`
4. When asked for the Git Repository URL, enter the URL of this repository: `https://github.com/rominirani/cloud-code-sample-repository.git`
5. You will shown the Python project. Select that.
6. Complete the rest of the steps to import the projects into Visual Studio Code. 

You are all set now and can directly deploy these APIs to a Serverless Compute Ennvironment like Cloud Run.

## Directly deploying the Inventory API to Cloud Run from the `python-flask-api` folder
From the `python-flask-api` folder, give the following gcloud command:
```
gcloud run deploy --source .
```
Follow the instructions at each step as part of the above interactive deployment at the command line/terminal.

## Injecting delay into Inventory Requests

If you are looking to simulate Service Level Objectives (SLO) Monitoring in Google Cloud Monitoring, you can setup a Latency SLO vis Service Monitoring on the Cloud Run service and introduce delays in the /inventory/<Product-ID> API endpoint to simulate eating into the Error Budget. To do that, ensure that you deploy a new version of the Cloud Run service by providing an environment variable in the Cloud Run service settings. Provide the environment variable: `delay` and with a value `True`. 

Note: This is available only in the `Python` version of the API.

## Injecting a failure in Uptime Checks

If you are looking to simulate an Uptime Check failure in Google Cloud Monitoring, simply provide an environment variable: `uptimecheckfailure` and with a value `True`. This will result in the `/healthy` endpoint returning an error. 



Note: This is available only in the `Python` version of the API.

