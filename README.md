# MoJ ClamAV Docker Container (Alpine)

Provide a ClamAV daemon running in a docker container, listening for connections on port 3310

This is a component of the [mojfile-uploader](https://github.com/ministryofjustice/mojfile-uploader) project.

## Usage

    docker build -t clamav .

    docker run -d -p 3310:3310 --name clamd clamav

## Push to MoJ repository

Adjust version number, as appropriate

    docker tag clamav registry.service.dsd.io/ministryofjustice/clamav:0.3.0

    docker push registry.service.dsd.io/ministryofjustice/clamav:0.3.0

## Database

This container will *always* fetch the latest database in the
foreground as the first thing it does when it run. It will not start if
this fails.

After it is running, `freshclam` continues to run as a daemon, and will
check for updates every two hours.

## Haproxy

This is used to ensure that a database update does not stop the av
service from responding.  Clamd blocks for around 20 seconds when it
reloads an updated database. While it is reloading, the clamav-rest client
errors. At the time of this comment this is happening frequently enough
that we have seen it occur with real users on the production service.
