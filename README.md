# Automated risk discovery using the Open Source tools

**Note: This page/gist will be available for A LIMITED TIME only. You should download the content of this page if you find it useful.** Thanks!

# Introduction
This exercise provides a quick introduction to how to automatically analyze the architectural health of your workloads running in your AWS account using the open-source tools [Steampiple](https://github.com/turbot/steampipe) and [Powerpipe](https://github.com/turbot/powerpipe) that will be used in this exercise. 
> (A BIG thanks! to [Turbot](https://turbot.com) Steampipe/Powerpipe collaborators for building such a wonderful tools)

![Steampipe](https://camo.githubusercontent.com/31d20af0f48b24499c76e768d4b4a7f1402cff7ee7fb9f73c019cb4a18893977/68747470733a2f2f737465616d706970652e696f2f696d616765732f737465616d706970652d636f6c6f722d6c6f676f2d616e642d776f72646d61726b2d776974682d77686974652d627562626c652e737667) 
![Powerpipe](https://powerpipe.io/images/powerpipe_wordmark_darkmode.svg)


# _Warning/Disclaimer_
- Participating in this exercise is **AT YOUR OWN RISK**. Run this only if you're comfortable with what you're going to do (i.e., setting up a Role, running code from a public, open source repository).
- Use it only on a test or development account. Don't run this on a production account unless you've fully tested it.

## Setup:
- Login to [AWS Console](https://console.aws.amazon.com)
- Start [AWS CloudShell](https://console.aws.amazon.com/cloudshell)
- #### Install Steampipe & AWS plugin

	```
	mkdir -p $HOME/aws/
	cd $HOME/aws/
	sudo /bin/sh -c "$(curl -fsSL https://steampipe.io/install/steampipe.sh)" && \
	steampipe plugin install aws
	#
	```

- #### Install Powerpipe & AWS mod *(ignore warning/s)*

	```
	sudo /bin/sh -c "$(curl -fsSL https://powerpipe.io/install/powerpipe.sh)" && \
	powerpipe mod init  && \
	powerpipe mod install github.com/turbot/steampipe-mod-aws-well-architected
	#
	```	
- #### Start Steampipe service

	```
	steampipe service start
	```
	*The output should confirm that Steampipe service is running:*

	```
	Steampipe service is running:
	
	Database:
	
	  Host(s):            127.0.0.1, ::1, 172.17.0.1, 10.130.81.152
	  Port:               9193
	  Database:           steampipe
	  User:               steampipe
	  Password:           ********* [use --show-password to reveal]
	  Connection string:  postgres://steampipe@127.0.0.1:9193/steampipe
	
	Managing the Steampipe service:
	
	  # Get status of the service
	  steampipe service status
	
	  # View database password for connecting from another machine
	  steampipe service status --show-password
	
	  # Restart the service
	  steampipe service restart
	
	  # Stop the service
	  steampipe service stop
	```

## Exercises:

### Run a Well-Architected benchmark in the CloudShell Console

- Start [AWS CloudShell](https://console.aws.amazon.com/cloudshell)
- #### Check if Steampipe service is running 

	```
	cd $HOME/aws/
	steampipe service status

	```

	* *In case of any error/s, repeat* **Setup** *section*

- #### Run the AWS Well-Architected Powerpipe benchmark
	
	```
	powerpipe benchmark run aws_well_architected.benchmark.well_architected_framework

	```

	You can run a benchmark focused on the **Security pillar**

	```
	powerpipe benchmark run aws_well_architected.benchmark.well_architected_framework

	```
	*The results will be displayed in the* **CloudShell Console**

### Export the benchmark output to html
- Start [AWS CloudShell](https://console.aws.amazon.com/cloudshell)
- #### Check if Steampipe service is running 

	```
	cd $HOME/aws/	
	steampipe service status

	```

	* *In case of any error/s, repeat* **Setup** *section*

- #### Run the AWS Well-Architected Powerpipe benchmark and export to html
	
	```
	powerpipe benchmark run \
	aws_well_architected.benchmark.well_architected_framework \
	--export wa_report.html
	```

	You can run a benchmark focused on the **Security pillar**

	```
	powerpipe benchmark run \
	aws_well_architected.benchmark.well_architected_framework_security \
	--export wa_security_report.html
	```
	*The benchmark results will be* **exported to the html file** *and displayed in the* **CloudShell Console**

- #### Download the exported html file/s

	- Click the `**[ Actions \/ ]**` menu (in blue) located at the top-right
	- Click in `Download file`
	- On the text box `Individual file path` specify the **absolute path** of the file to download. If you followed this exercise, you can use any of: 
		- `/home/cloudshell-user/aws/wa_security_report.html`
		- or
		- `/home/cloudshell-user/aws/wa_report.html`


### Clean-up
- Eliminate CloudShell environment
	- Click the `**[ Actions \/ ]**` menu
	- Click Delete.
	- Confirm
- _**NOTE:**_ This lab does not incur on any cost or resource deployment