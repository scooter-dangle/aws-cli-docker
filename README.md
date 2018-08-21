NOTE: Forked and modified from
[sekka1/aws-cli-docker](https://github.com/sekka1/aws-cli-docker). That one is
more likely to work.

# AWS CLI Docker Container
[![GitHub forks](https://img.shields.io/github/forks/scooter-dangle/aws-cli-docker.svg)](https://github.com/scooter-dangle/aws-cli-docker/network)
[![GitHub stars](https://img.shields.io/github/stars/scooter-dangle/aws-cli-docker.svg)](https://github.com/scooter-dangle/aws-cli-docker/stargazers)
[![GitHub issues](https://img.shields.io/github/issues/scooter-dangle/aws-cli-docker.svg)](https://github.com/scooter-dangle/aws-cli-docker/issues)
[![Twitter](https://img.shields.io/twitter/url/https/github.com/scooter-dangle/aws-cli-docker.svg?style=social)](https://twitter.com/intent/tweet?text=AWS%20CLI%20in%20a%20%40Docker%20container%20%40AWSCLI:&url=https://github.com/scooter-dangle/aws-cli-docker)
[![Docker Pulls](https://img.shields.io/docker/pulls/scoots/aws-cli-docker.svg)](https://hub.docker.com/r/scoots/aws-cli-docker/)
[![Docker Stars](https://img.shields.io/docker/stars/scoots/aws-cli-docker.svg)](https://hub.docker.com/r/scoots/aws-cli-docker/)


# Supported tags and respective `Dockerfile` links

- [`1.15.82` (*1.15.82/Dockerfile*)](https://github.com/scooter-dangle/aws-cli-docker/tree/1.15.82/Dockerfile)

# AWS CLI Version

* 1.15.82

# Build

```
docker build --tag scoots/aws-cli-docker:x.x .
```

# Description

Docker container with the AWS CLI installed.

Using [Alpine linux](https://hub.docker.com/_/alpine/).  The Docker image is 87MB

An automated build of this image is on Docker Hub: https://hub.docker.com/r/scoots/aws-cli-docker/

## Getting your AWS Keys:

[http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html#cli-signup](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html#cli-signup)

## Passing your keys into this container via environmental variables:

[http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-environment](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-environment)

## Command line options for things like setting the region

[http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-command-line](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html#cli-command-line)

## You can run any commands available to the AWS CLI

[http://docs.aws.amazon.com/cli/latest/index.html](http://docs.aws.amazon.com/cli/latest/index.html)

## Example Usage:

### Describe an instance:

    docker run \
    --volume ~/.aws:/root/.aws
    scoots/aws-cli-docker \
    ec2 describe-instances --instance-ids i-90949d7a

output:

    {
        "Reservations": [
            {
                "OwnerId": "960288280607",
                "ReservationId": "r-1bb15137",
                "Groups": [],
                "RequesterId": "226008221399",
                "Instances": [
                    {
                        "Monitoring": {
                            "State": "enabled"
                        },
                        "PublicDnsName": null,
    ...
    ...
    }

### Return a list of items in s3 bucket

    docker run \
    --volume ~/.aws:/root/.aws
    scoots/aws-cli-docker \
    s3 ls

output:

    2014-06-03 19:41:30 folder1
    2014-06-06 23:02:29 folder2

### Upload content of your current directory (say it contains two files _test.txt_ and _test2.txt_) to s3 bucket

    docker run \
    --volume ~/.aws:/root/.aws
    --volume $PWD:/data \
    scoots/aws-cli-docker \
    s3 sync . s3://mybucket

output:

    (dryrun) upload: test.txt to s3://mybucket/test.txt
    (dryrun) upload: test2.txt to s3://mybucket/test2.txt

doc: http://docs.aws.amazon.com/cli/latest/reference/s3/index.html

### Retrieve a decrypted Windows password by passing in your private key
We will map the private keys that resides on your local system to inside the container

    docker run \
    --volume ~/.aws:/root/.aws
    scoots/aws-cli-docker \
    ec2 get-password-data --instance-id  <<YOUR_INSTANCE_ID>> --priv-launch-key /tmp/key.pem

Output:

    {
        "InstanceId": "i-90949d7a",
        "Timestamp": "2014-12-11T01:18:27.000Z",
        "PasswordData": "8pa%o?foo"
    }

doc: http://docs.aws.amazon.com/cli/latest/reference/ec2/get-password-data.html

## Example Usage with Docker Compose:

    docker-compose run aws s3 ls
