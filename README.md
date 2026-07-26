# java-sapient-proto

Java classes for the SAPIENT
[BSI Flex 335 v2.0](https://github.com/dstl/SAPIENT-Proto-Files) protobuf
message definitions.

Python counterpart:
[py-sapient-proto](https://github.com/cafanasyev/py-sapient-proto).

The proto folder structure is re-arranged relative to upstream so the files
compile with standard protoc include paths (see
[dstl/SAPIENT-Proto-Files#13](https://github.com/dstl/SAPIENT-Proto-Files/pull/13)).

## Install

Requires Java 21+.

Maven:

```xml
<dependency>
    <groupId>io.github.cafanasyev</groupId>
    <artifactId>java-sapient-proto</artifactId>
    <version>2.0</version>
</dependency>
```

Gradle:

```kotlin
implementation("io.github.cafanasyev:java-sapient-proto:2.0")
```

To regenerate the classes locally instead: `./mvnw protobuf:generate`.

## Usage

```java
import com.google.protobuf.Timestamp;
import java.time.Instant;
import uk.gov.dstl.sapientmsg.bsiflex335v2.Alert;
import uk.gov.dstl.sapientmsg.bsiflex335v2.SapientMessage;

Instant now = Instant.now();
SapientMessage msg = SapientMessage.newBuilder()
        .setTimestamp(Timestamp.newBuilder()
                .setSeconds(now.getEpochSecond())
                .setNanos(now.getNano()))
        .setNodeId("f47ac10b-58cc-4372-a567-0e02b2c3d479")
        .setAlert(Alert.newBuilder()
                .setAlertId("01ARZ3NDEKTSV4RRFFQ69G5FAV")
                .setAlertType(Alert.AlertType.ALERT_TYPE_WARNING))
        .build();

byte[] data = msg.toByteArray();                          // class -> wire bytes
SapientMessage parsed = SapientMessage.parseFrom(data);    // wire bytes -> class
```

For JSON conversion (`JsonFormat`, analogous to Python's `google.protobuf.json_format`),
add `com.google.protobuf:protobuf-java-util` — it isn't a dependency of this
package, since not every consumer needs it.

**Also see:** [java-sapient-sdk](https://github.com/cafanasyev/java-sapient-sdk) —
a ready-made TCP client and node lifecycle manager built on these classes, so
you don't have to write one.

## License

This repository contains Protocol Buffer definitions from
https://github.com/dstl/SAPIENT-Proto-Files
which implement BSI Flex 335 © The British Standards Institution.

See [LICENSE](https://github.com/cafanasyev/java-sapient-proto/blob/master/LICENSE) and [third-party-licenses](https://github.com/cafanasyev/java-sapient-proto/tree/master/third-party-licenses)
(original .proto files are Dstl (c) Crown Copyright, governed by the terms in
`third-party-licenses/LICENCE.txt`).