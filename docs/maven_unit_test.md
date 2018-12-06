# Maven Unit Test

In order to provide Unit Test to our project, add to the pom.xml:
```console
<dependencies>
...
    <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId
         <version>4.11</version>
         <scope>test</scope>
    </dependency>
    <build>
         <finalName>clickCount</finalName>
         <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</dependencies>
```

