A cross-platform container for the [hawkBit Update Server](http://repo1.maven.org/maven2/org/eclipse/hawkbit/hawkbit-update-server/).
You can see more details about the container at https://hub.docker.com/r/hawkbit/hawkbit-update-server/

NOTE: The hawkBit Update Server .jar in this folder has been customized to allow anonymous access to the DDI (Direct Device Integration) API

## Build your own version of hawkBit Update Server

  * git clone https://github.com/eclipse/hawkbit
  * cd hawkbit
  * cp [this folder]/0001-properties-allow-anon-access-to-DDI-API.patch .
  * git am 0001-properties-allow-anon-access-to-DDI-API.patch
  * mvn clean install
  * cp hawkbit-runtime/hawkbit-update-server/target/hawkbit-update-server-*.jar [this folder]

## How to use this image

```
docker run -p 8080:8080 hub.foundries.io/hawkbit-update-server
```

Standard prebuilt versions of hawkBit Update Server are [here](http://repo1.maven.org/maven2/org/eclipse/hawkbit/hawkbit-update-server/)
