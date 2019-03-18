A cross-platform container for the Eclipse [Leshan](https://hudson.eclipse.org/leshan/job/leshan/) server

## How to use this image

```
docker run -p5683:5683/udp -p5684:5684/udp -p8080:8080/tcp hub.foundries.io/leshan
```

NOTE: To open the Leshan Web UI goto: http://localhost:8080
