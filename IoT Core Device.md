## IoT
- go on `Manage`
- click on `Things`
- click button `Register a thing`
- click button `Create a single thing`
- give it the name: `vehicle-turin-1`

## Create a Type for the thing
- Click button `Create a type` and open the new window:
- Name it `automobile`
- Describe it `A sensor for an automobile`
- Go ahead and click button `Create thing type`

## Create a Group for the thing
- Add the thing to a Group by clicking the button `Create group`
- Parent Group: `Groups/`
- Name: `Turin` 
- Description: `Vehicles in Turin` 
- Attribute Key: `city`
- Value: `Turin` 
- Click Button `Create thing group`



## Create Certificates
- Choose `IoT Core`
- click `manage`
- click `Register a thing`
- Add a certificate for your thing
- click `secure`
- click `Create a certificate`
- click `Create certificate`
- download:
`A certificate for this thing`
`A public key`
`A private key`
- download a root CA for AWS IoT:
- click `download` and choose `Amazon root CA1`
- click `Activate`
- click button `Attach a policy` and click button `Register Thing`

## Exit and create a policy after
- in the next window, click next and then go to the policy creation afterwards

## Create a policy
- go to `Secure`
- choose `Policies`
- click `Create a policy`
- Name it `vehicle-policy`
- define Action: `iot:*`
- define resource ARN: `*`
- define Effect: `Allow`
- click `Create`

## Attach the policy to the certificate
- go to `Secure` and then `Certificates`
- click the meatball menu and `Attach policy`
- tick the right `vehicle-policy` and click attach 

## Verify the Certificate has the right policy
- Open `Secure`
- Chose `Policies`





## IoT Authentication and Authorization

## IoT Message Broker
- Click button `Test`
- Click `Subscribe to a topic`
- Name the Subscription topic `TurinVehicle/1`
- click button `Subscribe to topic`
- click button `Publish to topic`
- download from 

## Create Python Veirtual Environment and launch client
- python -m venv venv
- cd venv/scripts
- activate
- cd ..
- cd ..
- pip install AWSIoTPythonSDK
- pip freeze
- python 
