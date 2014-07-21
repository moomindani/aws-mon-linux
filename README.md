# aws-mon-linux

Bash script that reports custom metric data about Linux performance to Amazon CloudWatch

This script is intended for use with Amazon EC2 instances running Linux operating systems. The scripts have been tested on the following Amazon Machine Images (AMIs) for both 32-bit and 64-bit versions:

* Amazon Linux 2014.3
* Red Hat Enterprise Linux 6.4
* Ubuntu Server 13.10

The script is written in simple Bash, so you can use it very easily. 
The script is almost compatible with [Amazon CloudWatch Monitoring Scripts for Linux](http://aws.amazon.com/code/8720044071969977), so you can use almost same options. 


## Metrics of This Script

This script can report the following metrics:

* load average
* interrupt
* context switch
* cpu (user/system/idle/wait/steal)
* memory
* swap
* disk

## Prerequisites

This script requires [AWS Command Line Interface](http://aws.amazon.com/cli/).

### Amazon Linux

Instances that you launch using the Amazon Linux AMI already include the CLI tools.

All you have to do is to install git.

```
yum install git
```


### RHEL, Ubuntu

#### RHEL

Install git.

```
yum install git
```

#### Ubuntu

Install git, unzip and jre.

```
apt-get install git
apt-get install unzip
apt-get install default-jre
```


#### RHEL, Ubuntu

You can install [AWS Command Line Interface](http://aws.amazon.com/cli/) with following steps:

```
pip install awscli
```


## Getting Started

Download "aws-mon.sh" or clone repository using following steps: 

```
git clone https://github.com/moomindani/aws-mon-linux.git
cd aws-mon-linux
./aws-mon.sh --help
```

## Using the Script

This script collects load average, cpu, memory, swap, and disk space utilization data on the current system. It then makes a remote call to Amazon CloudWatch to report the collected data as custom metrics.

### Option

Name                       | Description
-------------------------- | -------------------------------------------------
--help                     | Displays usage information.
--version                  | Displays the version number.
--verify                   | Checks configuration and prepares a remote call.
--verbose                  | Displays details of what the script is doing.
--debug                    | Displays information for debugging.
--from-cron                | Use this option when calling the script from cron.
--profile VALUE            | Use a specific profile from your credential file.
--load-ave1                | Reports load average for 1 minute in counts.
--load-ave5                | Reports load average for 5 minutes in counts.
--load-ave15               | Reports load average for 15 minutes in counts.
--interrupt                | Reports interrupt in counts.
--context-switch           | Reports context switch in counts.
--cpu-us                   | Reports cpu utilization (user) in percentages.
--cpu-sy                   | Reports cpu utilization (system) in percentages.
--cpu-id                   | Reports cpu utilization (idle) in percentages.
--cpu-wa                   | Reports cpu utilization (wait) in percentages.
--cpu-st                   | Reports cpu utilization (steal) in percentages.
--memory-units UNITS       | Specifies units in which to report memory usage. If not specified, memory is reported in megabytes. UNITS may be one of the following: bytes, kilobytes, megabytes, gigabytes.
--mem-used-incl-cache-buff | Count memory that is cached and in buffers as used.
--mem-util                 | Reports memory utilization in percentages.
--mem-used                 | Reports memory used in megabytes.
--mem-avail                | Reports available memory in megabytes.
--swap-util                | Reports swap utilization in percentages.
--swap-used                | Reports allocated swap space in megabytes.
--swap-avail               | Reports available swap space in megabytes.
--disk-path PATH           | Selects the disk by the path on which to report.
--disk-space-units UNITS   | Specifies units in which to report disk space usage. If not specified, disk space is reported in gigabytes. UNITS may be one of the following: bytes, kilobytes, megabytes, gigabytes.
--disk-space-util          | Reports disk space utilization in percentages.
--disk-space-used          | Reports allocated disk space in gigabytes.
--disk-space-avail         | Reports available disk space in gigabytes.
--disk-metric-suffix       | Suffix to add to disk metrics.
--all-items                | Reports all items.

### Examples

The following examples assume that you have already configured awscli with "aws configure". 

#### To perform a simple test run without posting data to CloudWatch

* Run the following command:

```
./aws-mon.sh --mem-util --verify --verbose
```

#### To collect all available cpu metrics and send them to CloudWatch

* Run the following command:

```
./aws-mon.sh --cpu-us --cpu-sy --cpu-id --cpu-wa --cpu-st
```

#### To collect all available memory metrics and send them to CloudWatch

* Run the following command:

```
./aws-mon.sh --mem-util --mem-used --mem-avail
```

#### To collect all available disk metrics and send them to CloudWatch

* Run the following command:

```
./aws-mon.sh --disk-space-util --disk-space-used --disk-space-avail --disk-path /
```

#### To collect all load average metrics and send them to CloudWatch

* Run the following command:

```
./aws-mon.sh --load-ave1 --load-ave5 --load-ave15
```

#### To set a cron schedule for metrics reported to CloudWatch

1. Start editing the crontab using the following command:

```
crontab -e
```

2. Add the following command to report all items to CloudWatch every five minutes:

```
*/5 * * * *  ~/aws-mon-linux/aws-mon.sh --all-items --disk-path=/ --from-cron
```
