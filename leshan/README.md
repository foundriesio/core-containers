# LWM2M Leshan

## Build the container

```
docker build -t leshan --force-rm -f Dockerfile .
```

## Run the container

```
docker run --restart=always -d -t --net=host --read-only --tmpfs=/tmp --name leshan leshan
```

## Run the pre-built container

```
docker run --restart=always -d -t --net=host --read-only --tmpfs=/tmp --name leshan opensourcefoundries/leshan:latest
```
