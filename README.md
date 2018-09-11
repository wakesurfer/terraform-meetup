
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
6. Add the configuration details for your instance to Terraforms environment script
7. Test the Terraform sample scripts

## 1. Install Terraform on your machine ##
Download Terraform from https://www.terraform.io/downloads.html
Terraform is distributed in binary form for all platforms. Extract the binary from the archive in a directory on your machine.
Next step is to install the Terraform provider plugin for the type of cloud provider you will use, in our case the Oracle Terraform provider. You can download the provider from GitHub https://github.com/oracle/terraform-provider-oci.

Unpack the provider archive and copy the executable to (replace USERNAME with your username)
```
C:\Users\USERNAME\AppData\Roaming\terraform.d\plugins\terraform-provider-oci_v2.2.2.exe
```

## 2. Install some form of bash on your machine ##
We need to run some OpenSSL commands to create the keys used for authentication. OpenSSL can be installed in most Linux's via the package manager and used from the shell. The easiest way in Win 10 is probably to install the Ubuntu bash or similar from the Windows App Store.
Another possibility is to install the Git-bash https://gitforwindows.org.

If you choose to download the pre-created keys then you can skip this step.

## 3. Copy the demo and environment files and setup your Terraform working directory ##
Create a working directory for your lab and copy the demo and environment archive from GitHub either cloning the repository or downloading the archive https://github.com/wakesurfer/terraform-meetup/blob/master/single_and_demo.tgz

## 4. Get the Credentials for an instance in the Oracle Cloud (provided at the meetup) ##
The credentials will be posted to some suitable place so you don't have to type them in manually.

## 5. Create the public/private keys required to access the Oracle Cloud ##
Create the public/private keys via your preferred tool as per below.
Or as an alternative download a set of pre-created keys from here
https://github.com/wakesurfer/terraform-meetup/blob/master/oci-keys.zip

Additional instructions on managing keys can be found here https://docs.cloud.oracle.com/iaas/Content/API/Concepts/apisigningkey.htm#How

To create the private key with a password.
```
openssl genrsa -out ~/.oci/oci_api_key.pem -aes128 2048
```

To generate the public key
```
openssl rsa -pubout -in ~/.oci/oci_api_key.pem -out ~/.oci/oci_api_key_public.pem
```

Now you need to add the public key to your cloud instance. The private key will be added to your Terraform setup scripts soon.

Login to Oracle OCI Cloud.
Go to Menu/Identity/Users and select your user.
On your users information page click "Add Public Key"
Add the contents of the public key file you created earlier "~/.oci/oci_api_key_public.pem".

If you need more help with the steps above [more details can be found here](https://docs.cloud.oracle.com/iaas/Content/API/Concepts/apisigningkey.htm#How2).

## 6. Add the configuration details for your cloud instance to Terraforms environment script ##
In the Terraform samples archive there is an environment file for Windows Powershell "env-vars.ps1".
The following variables (OCID's etc) in this file needs to be updated to reflect your instance details.
Note: If you copied the key files the password is "abba"
```
### Substitute USERNAME to correct the path
### Substitute PASSWORD to the one you set on your private key
### Substitute the OCIDs, fingerprints and keys with the correct ones for your environment
### Authentication details
$env:TF_VAR_tenancy_ocid="ocid1.tenancy.oc1..aaaaaaaazabhxa2ah2t3kwOx6azvo2tl7myjxzq2jdxjf2tabn6mndnrh5lq"
$env:TF_VAR_user_ocid="ocid1.user.oc1..aaaaaaaaehgxm9ccb4locxlxfdfsb4ukslipabcgvoy2gmbencb53axtd3hq"
$env:TF_VAR_fingerprint="04:5d:ce:54:64:4b:28:23:c4:e5:2d:f8:46:86:3f:49"
$env:TF_VAR_private_key_path="C:\Users\USERNAME\.oci\oci_api_key.pem"
$env:TF_VAR_private_key_password="PASSWORD"

### Compartment
$env:TF_VAR_compartment_ocid="ocid1.compartment.oc1..aaaaaaaaicsaq45yn5l6idotcgzhpiwy6ljv4p6koswheo4h5wmguly3tbia"

### Region
$env:TF_VAR_region="us-ashburn-1"

```

## 7. Test the Terraform sample scripts that you have downloaded earlier ##
Execute the script to set the environment or you could close the Powershell and start a new instance.
```
.\env-vars.ps1
```

Finally you need to create a Terraform config file to instruct Terraform to perform some action.
So create the file vcn.tf in your working directory with a simple instruction to create a virtual network.
```
variable "tenancy_ocid" {}
variable "user_ocid" {}
variable "fingerprint" {}
variable "private_key_path" {}
variable "private_key_password" {}
variable "compartment_ocid" {}
variable "region" {}

provider "oci" {
  tenancy_ocid     = "${var.tenancy_ocid}"
  user_ocid        = "${var.user_ocid}"
  fingerprint      = "${var.fingerprint}"
  private_key_path = "${var.private_key_path}"
  private_key_password = "${var.private_key_password}"
  region           = "${var.region}"
}

resource "oci_core_virtual_network" "vcn1" {
  cidr_block     = "10.0.0.0/16"
  dns_label      = "vcn1"
  compartment_id = "${var.compartment_ocid}"
  display_name   = "My Network"
}
```

Now you should be able to test your Terraform installation by running
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

To clean up and remove the things you created with "terraform apply"
```
terraform destroy
```
