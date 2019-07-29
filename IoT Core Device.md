## Create Certificates
- Choose `IoT Core`
- click `manage`
- click `Register a thing`
- Add a certificate for your thing
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
- Name it `sensor-policy`
- define Action: `iot:*`
- define resource ARN: `*`
- define Effect: `Allow`
- click `Create`

## Attach the policy to the certificate
- go to `Secure` and then `Certificates`
- click the meatball menu and `Attach policy`
- tick the right `sensor policy` and attach

## Verify the Certificate has the right policy
- Open `Certificate`
- Chose `Policies`


## IoT

- Register a thing
- Create a single thing
- give it the name: `store-sensor-seattle-1`

- Click button `Create a type` and open the new window:
- Name it `sensor`
- Describe it `A sensor for a store`
- Go ahead and `Create thing type`

- Add the thing to a Group by creating it in the new window
- Click button `Create a thing group`
- Parent Group: `Groups/`
- Name: `Seattle`
- Description: `Things in Seattle`
- Attribute Key: `city`
- Value: `Seattle`
- Click Button `Create thing group`

## IoT Authentication and Authorization
