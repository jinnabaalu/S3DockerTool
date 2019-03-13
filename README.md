# S3 Docker Tool for s3tool - Docker container that periodically backups files to Amazon S3 using s3cmd



[![nodesource/node](http://dockeri.co/image/jinnabalu/s3dockertool)](https://registry.hub.docker.com/u/jinnabalu/s3dockertool/)

## Amazon S3 Tools: Command Line S3 Client Software and S3 Backup as a docker container

 Detailed explanation here on [S3Tools](http://s3tools.org/usage) 

## Parameters

* `-e ACCESS_KEY=<AWS_KEY>`: Your AWS ACCESS_KEY
* `-e SECRET_KEY=<AWS_SECRET>`: Your AWS SECRET_KEY
* `-e S3_PATH=s3://<BUCKET_NAME>/<PATH>/`: S3 Bucket name and path. Should end with trailing slash.
* `-v /path/to/backup:/data:ro`: mount target local folder to container's data folder. Content of this folder will be synced with S3 bucket.

### Optional parameters

* `-e PARAMS="--dry-run"`: parameters to pass to the sync command ([full list here](http://s3tools.org/usage)).
* `-e DATA_PATH=/data/`: container's data folder. Default is `/data/`. Should end with trailing slash.
* `-e 'CRON_SCHEDULE=0 1 * * *'`: specifies when cron job starts ([details](http://en.wikipedia.org/wiki/Cron)). Default is `0 1 * * *` (runs every day at 1:00 am).
* `no-cron`: run container once and exit (no cron scheduling).

### Examples

Run an upload to S3 everyday at 12:00pm:

    docker run -d \
        -e ACCESS_KEY=myawskey \
        -e SECRET_KEY=myawssecret \
        -e S3_PATH=s3://my-bucket/backup/ \
        -e 'CRON_SCHEDULE=0 12 * * *' \
        -v /home/user/data:/data:ro \
        jinnabalu/s3dockertool

Run once, then delete the container:

    docker run --rm \
        -e ACCESS_KEY=myawskey \
        -e SECRET_KEY=myawssecret \
        -e S3_PATH=s3://my-bucket/backup/ \
        -v /home/user/data:/data:ro \
        jinnabalu/s3dockertool no-cron

Run once to get a backup from S3, then delete the container:

    docker run --rm \
        -e ACCESS_KEY=myawskey \
        -e SECRET_KEY=myawssecret \
        -e S3_PATH=s3://my-bucket/backup/ \
        -v /home/user/data:/data:rw \
        jinnabalu/s3dockertool get

Run once to delete the backup from s3, then delete the container:

    docker run --rm \
        -e ACCESS_KEY=myawskey \
        -e SECRET_KEY=myawssecret \
        -e S3_PATH=s3://my-bucket/backup/ \
        jinnabalu/s3dockertool delete

### Security considerations
* On restore, this opens up permissions on the restored files widely
