# Zenko Swarm Test

Some notes and code based on this repo : https://github.com/scality/Zenko/tree/master/swarm-testing

and inspired by the recent Docker Dublin Meetup : https://www.meetup.com/Docker-Dublin/events/250786268/

## What's new ?

This is the first time playing about with Zenko and I just wanted to get something working.

Followed Laure's instructions but used the docker-stack.yml and config.json in this repo.  This will create an attachable overlay network. The s3 and lb services are connected to the overlay.  This requires at least docker-compose schema v3.2.

## Build a container image with awscli installed

```
docker build -t dev-env -f Dockerfile.dev-env .
```

## Start the container and attach to overlay network

```
docker run -it --name zenko --network zenko-testing_zenko-net dev-env
```

## Test that it worked

export AWS_ACCESS_KEY_ID=accessKey1
export AWS_SECRET_ACCESS_KEY=verySecretKey1

echo "HelloWorld" > test.txt

aws s3 --endpoint http://lb mb s3://bucket1 --region=us-east-1
make_bucket: bucket1

aws s3 --endpoint http://lb ls
2018-06-09 16:42:58 bucket1

aws s3 --endpoint http://lb cp test.txt s3://bucket1
upload: ./test.txt to s3://bucket1/test.txt

aws s3 --endpoint http://lb ls s3://bucket1
2018-06-09 17:36:10       1510 test.txt 


