## An example for build-variant-configurable dependencies


The key part of this solution is to set `publishNonDefault` to `true` in the library's build.gradle:

In the library's build.gradle:

```groovy
apply plugin: 'com.android.library'

android {
    compileSdkVersion 27
    buildToolsVersion "27.0.3"
    publishNonDefault true

    ...
    
    buildTypes {
        qa {
            debuggable true
        }
    }
}
```


In the app's build.gradle:

```groovy
apply plugin: 'com.android.application'

android {
    compileSdkVersion 27
    buildToolsVersion '27.0.3'

    ...
    
    buildTypes {
        debug {
            signingConfig signingConfigs.debug
        }
        release {
            signingConfig signingConfigs.debug
        }
        qa {
            signingConfig signingConfigs.debug
        }
    }
}

dependencies {

    ...
    
    debugCompile project(path: ':lib', configuration: 'debug')
    releaseCompile project(path: ':lib', configuration: 'release')
    qaCompile project(path: ':lib', configuration: 'qa')
}

```





[See this wonderful answer on StackOverflow](https://stackoverflow.com/a/24910671/1335312)
