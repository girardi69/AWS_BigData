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

## Create a policy
- go to `Secure`
- choose `Policies`
- click `Create a policy`
- Name it `sensor-policy` vehicle-policy
- define Action: `iot:*`
- define resource ARN: `*`
- define Effect: `Allow`
- click `Create`

## Attach the policy to the certificate
- go to `Secure` and then `Certificates`
- click the meatball menu and `Attach policy`
- tick the right `sensor policy` and click attach vehicle-policy

## Verify the Certificate has the right policy
- Open `Certificate`
- Chose `Policies`


## IoT

- go on `Manage`
- click on `Things`
- click button `Register a thing`
-Click button `Create a single thing`
- give it the name: `store-sensor-seattle-1` -- vehicle-turin-1

- Click button `Create a type` and open the new window:
- Name it `sensor` -- vehicle
- Describe it `A sensor for a store`
- Go ahead and click button `Create thing type`

## Add this thing to a group
- Add the thing to a Group by clicking the button `Create group`
- Parent Group: `Groups/`
- Name: `Seattle` Turin
- Description: `Things in Seattle` -- Vehicles in Turin
- Attribute Key: `city`
- Value: `Seattle` Turin
- Click Button `Create thing group`

## IoT Authentication and Authorization

## IoT Message Broker
- Click button `Test`
- Click `Subscribe to a topic`
- Name the Subscription topic `TurinVehicle/1`
- click button `Subscribe to topic`
- click button `Publish to topic`
- download from 

