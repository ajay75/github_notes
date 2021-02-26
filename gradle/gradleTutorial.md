### Gradle build

## What gradle Java Plugin Do and how to apply?

     - Make Gradle "Java Aware"
     - Defines a set of tasks for Java build
     - Clean
     - Compile
     - Assemble
     - Test
- Simply in build.gradle file add a line
```
apply plugin: 'java'

```
## How to add Java Dependencies to Gradle?
 - First need to tell gradle where to get those dependencies, it could be mavencentral or nexus repo, add to the respository section
 ```
repositories{
    mavenCenteral()
}
```
- Add source folder to gradle:
```
sourceSets{
    main{
        java.srcDirs = ['src']
    }
}
dependencies{
    compile 'com.google.code.gson:gson:2.8.0'
}

# To add gradle wrapper to project
```
gradle wrapper --gradle-version 3.5
```