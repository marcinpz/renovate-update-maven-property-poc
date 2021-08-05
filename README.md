# renovate-update-maven-property-poc
POC for renovate updates maven vars, see: https://github.com/renovatebot/renovate/discussions/11104

## TODO
I would like `Renovate` updated `mq.version` from `pom.xml` as it is indirectly (via `MQ_VERSION`) used as a version for `rabbitmq` (see: `src/main/resources/docker-compose.yaml`). What `Renovate` config would work for that?

## Running

By default newest (updated by Renovate) image should be used, so:
```shell
mvn integration-test
```

-> would use MQ `v3.8` (or whatever it actually there in `mq.version` property)

But I need also to be able to use specific version with (and that is why I am using property + env. variable):
```shell
mvn integration-test -Dmq.version=3.7
```
-> runs MQ `v3.7`

## Possible workaround
I was able to fix this issue by using jar artifacts along with docker service. 
So add something like this into `pom.xml`:
```xml
<dependency>
    <groupId>an.group</groupId>
    <artifactId>rabbitmq-jar</artifactId>
    <version>${mq.version}</version>
    <scope>provided</scope>
</dependency>
```

- but the problem is that I need this jar and with same version (`rabbitmq-jar` in this sample).
