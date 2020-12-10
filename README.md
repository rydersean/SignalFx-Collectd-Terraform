# SignalFx-Collectd-Terraform
Cloning this git repository will allow you to use terraform to deploy a micro GCP instance (Ubuntu 18.04.5 LTS) with collectd installed and sending metrics to your SignalFx account. 

# Assumptions

* You have a GCP account
* You have terraform installed and working
* You have an active Splunk SignalFx account (Can be a trial account)

# Adding permissions for terraform to create resources on GCP

Log into your GCP account > Choose a project from the dropdown or Create a new project > Give your project or name or accept the default > Create.

Now you need to create a service account. In the GCP dashboard > IAM & admin > Service accounts > CREATE SERVICE ACCOUNT > Service Account Name [terraform-sa] > Create > Select a role > Project > Editor > Continue > Create Key > Key Type [json] > Create.

Download your project key and place it in the secrets/ directory. Then change: credentials = file("secrets/My_Project_7847-b6797c6771e2.json") to your project json filename in the main.tf file.

# Deploying with terraform

You can add your token before deploying through terraform (recommended) by changing "YOUR_SFX_ACCESS_TOKEN" to your token key in the scripts/installSignalFxCollectd.txt file.

Also add you SFx access token to the secrets/token file.

Change the GCP account key to your account key/json file in main.tf
credentials = file("secrets/My_Project_7847-b6797c6771e2.json")

Change the main.tf change YOUR_USER to your user
 metadata = {
   ssh-keys = "YOUR_USER:${file("~/.ssh/id_rsa.pub")}"
 }

$ terraform init

$ terraform apply

Answer 'yes' to deploy.

# Checking your collectd is running

$ tail -30f /var/log/syslog

Check for cpu.utilization and your instance hostname in a chart in SFx

---

# Cleaning up when you are done'

When you are done, you can destroy your deployment using the following command.

$ terraform destroy

Answer 'yes' to destroy the environment.
