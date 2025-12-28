chunk #2

I submitted a PR to the Apache Camel project after discovering that the Telegram component did not support voice
messages in the incoming message API.

While waiting for the review, merge, and release, I investigated how to override the patched component version in my
project pom.xml while keeping all other dependencies unchanged:

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>${quarkus.platform.group-id}</groupId>
            <artifactId>quarkus-camel-bom</artifactId>
            <version>${quarkus.platform.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <!-- Override ONLY camel-telegram -->
        <dependency>
            <groupId>org.apache.camel</groupId>
            <artifactId>camel-telegram</artifactId>
            <version>4.17.0-SNAPSHOT</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```

It also required adding a workaround to the build pipeline to build and install the patched component locally before running the main Maven build:

```yaml
- name: Checkout patched Camel
  uses: actions/checkout@v5
  with:
    repository: crionuke/camel
    ref: tg-voice-message
    path: camel
- name: Build & install patched camel-telegram
  run: |
    cd camel
    `mvn -DskipTests -pl components/camel-telegram -am install`
```

Link to the PR:
https://github.com/apache/camel/pull/20606

#chunks #camel #telegram #maven