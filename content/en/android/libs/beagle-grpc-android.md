---
title: Beagle gRPC for Android
weight: 4
description: 'Here you will find the **beagle-grpc-android** library to help you use gRPC in a project using Beagle in Android.
This lib will hold almost all necessary configuration to add gRPC into a Beagle Android project.'
---

---

![Maven Central](https://img.shields.io/maven-central/v/br.com.zup.beagle/beagle-grpc?color=green&label=Beagle-gRPC)
![**Maven Central**](https://img.shields.io/maven-central/v/br.com.zup.beagle/android?label=Beagle)

### Requirements

<hr>

Before you start to configure Beagle for your Android system, it’s important to check out if you have installed all the current versions of the following programs: ‌

- JDK 11+ language
- Android API level: at least 19 or higher
- Kotlin 1.5+

### Getting started

<hr>

This tutorial will configure Beagle from the start.

#### **Step 1:** Add a configuration on the Gradle file in your project:

- `Add mavenCentral()` into the `All projects` block on your Project Gradle file(build.gradle)<br>

```kotlin
allprojects {
    repositories {
        google()
        jcenter()
        mavenCentral()
    }
}
```

#### **Step 2:** Add Kotlin Kapt plugin and 3 dependencies on your application dependency manager (gradle.gradle)<br>

![Maven Central](https://img.shields.io/maven-central/v/br.com.zup.beagle/beagle-grpc?color=green&label=Beagle-gRPC)
![Maven Central](https://img.shields.io/maven-central/v/br.com.zup.beagle/android?label=Beagle)

- Add the packagingOptions code block to the build.gradle file, as in the example below:

- _The version numbers listed below may have been updated. Please check the actual versions on the tags above:_

```kotlin
plugins {
    id 'com.android.application'
    id 'kotlin-android'
    id 'kotlin-android-extensions'
    id 'kotlin-kapt'
}

android {
    kotlinOptions {
        jvmTarget = JavaVersion.VERSION_11
    }

    packagingOptions {
        pickFirst 'META-INF/LICENSE.md'
        pickFirst 'META-INF/LICENSE-notice.md'
    }
}

dependencies {
    kapt "br.com.zup.beagle:android-processor:${beagleVersion}"
    implementation "br.com.zup.beagle:beagle-grpc:${beagleVersion}"
}
```

#### **Step 3:** Create an AppHttpClientFactory

- This class will be responsible to provide the `HttpClient` that will be used by Beagle to perform requests.<br>

```
const val BASE_URL = "http://10.0.2.2:8080" // your gRPC host

@BeagleComponent
class AppHttpClientFactory: HttpClientFactory {
    override fun create(): HttpClient {
        return GrpcClient(BASE_URL, HttpClientDefault())
    }
}
```

Here, you are telling Beagle to use `GrpcClient` to perform network requests.<br><br>
Let's look to the constructor params of `GrpcClient`: <br>

- `grpcAddress`: Host of your gRPC server. Every request pointing to this host will be performed by `GrpcClient`.
- `customHttpClient`: All requests made to another host will be performed by this `HttpClient`.

Therefore, a request to `http://10.0.2.2:8080/foo` will be performed by `GrpcClient` and a request to another host, for example `http://127.0.1.1:5555/bar` will be performed by `HttpClientDefault`.
<br>

The `HttpClientDefault` implementation can be found at Beagle-Default lib.

<hr>

##### For more on Beagle, check out the [repository](https://github.com/ZupIT/beagle).

##### For more on Beagle-Defaults, check out [Beagle-Default]({{< ref path="/android/libs/beagle-defaults" lang="en" >}}) section.
