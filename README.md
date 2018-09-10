
----

# DiscoTech - Terraform meetup #

Welcome to the lab part of this meetup [Let's Meetup and script things](https://www.meetup.com/DiscoTech-By-Oracle/events/252266605/ "Let's Meetup and script things")

## Introduction	##

The labs are in tutorial form to help you learn and get an understanding of how Terraform works in controlling a cloud environment, in this case the Oracle OCI Cloud environment but it is basically the same for Amazon, Google, Microsoft etc.

To get going you need to do some setup tasks.

1. Install Terraform on your machine
2. Install some form of bash on your machine
3. Copy the demo and environment files and setup your Terraform working directory
4. Get the Credentials for an instance in the Oracle Cloud (provided at the meetup)
5. Create the public/private keys required to access the Oracle Cloud
6. Test the Terraform sample scripts

## Copy the demo and environment files and setup your Terraform working directory ##
Download and unpack the demo and environment files from here (terraform-meetup/single_and_demo.tgz)

## Install Terraform on your machine ##
Download Terraform from https://www.terraform.io/downloads.html
Terraform is distributed in binary form for all platforms. Extract the binary from the archive in a suitable directory on your machine.

## Install some form of bash on your machine ##
We need to run some OpenSSL commands to create the keys used for authentication. OpenSSL can be installed in most Linux's via the package manager and used from the shell. The easiest way in Win 10 is probably to install the Ubuntu bash or similar from the Windows App Store.
Another possibility is to install the Git-bash https://gitforwindows.org
Yet another possibility is installing Cygwin in Windows.

## Copy the demo and environment files and setup your Terraform working directory ##
Create a working directory for your lab and copy the demo and environment archive from GitHub either cloning the repository or downloading the file terraform-meetup/single_and_demo.tgz

## Get the Credentials for an instance in the Oracle Cloud (provided at the meetup) ##
The credentials will be posted to some suitable place so you don't have to type them in manually

## Create the public/private keys required to access the Oracle Cloud ##
Create the public/private keys via your preferred tool as per below or download a set of keys from here
https://github.com/wakesurfer/terraform-meetup/blob/master/oci-keys.zip

More instructions on managing keys can be found here https://docs.cloud.oracle.com/iaas/Content/API/Concepts/apisigningkey.htm#How

To create the private key
```
openssl genrsa -out ~/.oci/oci_api_key.pem -aes128 2048
```

To generate the public key
```
openssl rsa -pubout -in ~/.oci/oci_api_key.pem -out ~/.oci/oci_api_key_public.pem
```

Now you need to add the public key to your cloud instance
Todo



## Test the Terraform sample scripts that you downloaded earlier ##
To test your Terraform installation run
```
terraform init
```

To validate your Terraform script but not actually run it
```
terraform plan
```

And finally to run your Terraform script do
```
terraform apply
```

To clean up and remove the things you created with "terraform plan"
```
terraform destroy
```
