# aws-mon-linux

Bash script that reports custom metric data about Linux performance to Amazon CloudWatch

This script is intended for use with Amazon EC2 instances running Linux operating systems. The scripts have been tested on the following Amazon Machine Images (AMIs) for both 32-bit and 64-bit versions:

* Amazon Linux 2014.3
* Red Hat Enterprise Linux 6.4
* Ubuntu Server 13.10
* SuSE Linux Enterprise Server 11 sp3

This script is written in Bash and does not need any libraries.


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

This script requires [Amazon CloudWatch Command Line Tool](http://aws.amazon.com/developertools/2534).

NOTE: Amazon Linux already includes the Amazon EC2 CLI tools

```
# wget http://ec2-downloads.s3.amazonaws.com/CloudWatch-2010-08-01.zip
# unzip CloudWatch-2010-08-01.zip
# mkdir -p /opt/aws/apitools/
# mv CloudWatch-1.0.20.0 /opt/aws/apitools/mon-1.0.20.0
# cd /opt/aws/apitools
# ln -s mon-1.0.20.0 mon
# mkdir /opt/aws/bin
# cd /opt/aws/bin
# ln -s /opt/aws/apitools/mon-1.0.20.0/bin/mon-put-data mon-put-data
```


## Getting Started

Download "mon-aws.sh" or clone repository using following steps: 

```
# git clone https://github.com/moomindani/aws-mon-linux.git
# cd aws-mon-linux
# ./aws-mon.sh --help
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
--aws-credential-file PATH | Provides the location of the file containing AWS credentials. This parameter cannot be used with the --aws-access-key-id and --aws-secret-key parameters.
--aws-access-key-id VALUE  | Specifies the AWS access key ID to use to identify the caller. Must be used together with the --aws-secret-key option. Do not use this option with the --aws-credential-file parameter.
--aws-secret-key VALUE     | Specifies the AWS secret access key to use to sign the request to CloudWatch. Must be used together with the --aws-access-key-id option. Do not use this option with --aws-credential-file parameter.
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
--all-items                | Reports all items.

### Examples

The following examples assume that you have already updated the awscreds.conf file with valid AWS credentials. If you are not using the awscreds.conf file, provide credentials using the --aws-access-key-id and --aws-secret-key arguments.

#### To perform a simple test run without posting data to CloudWatch

* Run the following command:

```
# ./aws-mon.sh --mem-util --verify --verbose
```

#### To collect all available memory metrics and send them to CloudWatch

* Run the following command:

```
# ./aws-mon.sh --mem-util --mem-used --mem-avail
```

#### To set a cron schedule for metrics reported to CloudWatch

1. Start editing the crontab using the following command:

```
# crontab -e
```

2. Add the following command to report memory and disk space utilization to CloudWatch every five minutes:

```
*/5 * * * * ~/aws-mon.sh --mem-util --disk-space-util --disk-path=/ --from-cron
```
