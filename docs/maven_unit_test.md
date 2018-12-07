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

Unit testing is just a specialized form of automated testing

My goal is to have unit tests that verify all the main functionality of an app. It doesn’t prevent all bugs, but it does verify that the core of my app will always work. I can push changes with confidence. No more worrying about angry calls and emails!


“Imagine being able to make any change you want in your code and know that you did not break something.” ~ Sandi Metz

 Write unit tests – which are critical to ensuring long-term quality and correctness.
 
# database inserting 
