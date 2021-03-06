## What is gnip4j?

Gnip4j is an Open Source (ASFL 2.0) library to access and process Activities (tweets)
from the [Gnip API](http://support.gnip.com/)
from the data from the Java language.


## Features
   * 100% Pure Java - works on any Java Platform version 1.8 or later 
   * Minimal dependencies: 
      * The [Jackson JSON library](http://jackson.codehaus.org/) is the only requiered dependecy
      * Optionally if `slf4-api` is present in the classpath, logging is supported.
   * Out-of-the-box gzip support
   * Push model via Streaming (asynchronous processing)
   * Error handling support (and exponential-backoff reconnections)
   * Monitoring Support (JMX)
   * Enterprise Data Collector Support
   * Powertrack V1 and V2 support

## How To use gnip4j

For more information refer to the Gnip4j Reference Guide. Available formats:
 
   * [https://github.com/zauberlabs/gnip4j/wiki/Reference]
 
## Maven Integration

Add to your dependency Managment:

```xml
     <dependencyManagement>
       <dependencies>
        <dependency>
           <groupId>com.zaubersoftware.gnip4j</groupId>
           <artifactId>gnip4j-core</artifactId>
           <version>${gnip4j.version}</version>
        </dependency>
        <dependency>
           <groupId>com.zaubersoftware.gnip4j</groupId>
           <artifactId>gnip4j-http</artifactId>
           <version>${gnip4j.version}</version>
        </dependency>
      </dependencies>
     </dependencyManagement>
     ...
     <properties>
         <gnip4j.version>2.0.0</gnip4j.version>
     </properties>
```

and add to your project: 

```xml
    <dependencies>
       <dependency>
        <groupId>com.zaubersoftware.gnip4j</groupId>
        <artifactId>gnip4j-core</artifactId>
       </dependency>
    </dependencies>
```

If you need logging adding `slf4j` as a dependency.
 
## Code Snippet

### Consuming ten raw Activities from the stream
   
```java
    final AtomicInteger counter = new AtomicInteger();

    final DefaultGnipFacade x = DefaultGnipFacade.createPowertrackV2(…);
    final GnipStream stream = x.createPowertrackStream(String.class)
            .withAccount("YourAccount")
            .withType("prod")
            .withUnmarshall(new StringUnmarshaller())
            .withObserver(new StreamNotificationAdapter<String>() {
                @Override
                public void notify(final String activity, final GnipStream stream) {
                    System.out.println(activity);
                    if (i >= 10) {
                        stream.close();
                    }
                }
            })
            .build();
    stream.await();
```

## How To Contribute

[Send pull requests](http://help.github.com/pull-requests/) using GitHub.

Had an Issue? Fill it in https://github.com/zauberlabs/gnip4j/issues .

## Get in Contact

If you want to get in contact with us, or people interested in the project, please visit our group at
http://groups.google.com/group/gnip4j

## License

Copyright (c) 2011-2016 Zauber S.A. <http://www.zaubersoftware.com/>

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

gnip4j includes portions of software from licenced by the Apache Fundation (package
`com.zaubersoftware.gnip4j.api.stats.commonsio`).

## Code Conventions and Housekeeping

None of these is essential for a pull request, but they will all help.  They
can also be added after the original pull
request but before a merge.

* Use the project code format conventions. Import `XXXX.xml` from the root of
  the project if you are using Eclipse. Checkstyle configuration is available 
  at `XXXX.xml`
* Make sure all new .java files to have a simple Javadoc class comment with at
  least an @author tag identifying you, and preferably at least a paragraph on 
  what the class is for.
* Add the ASF license header comment to all new .java files (copy from existing 
  files in the project)
* Add yourself as an @author to the .java files that you modify substantially
  (more than cosmetic changes).
* A few unit tests would help a lot as well - someone has to do it.
* If no-one else is using your branch, please rebase it against the current
  master (or other target branch in the main project).
