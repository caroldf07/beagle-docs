---
title: Beagle gRPC para Android
weight: 4
description: 'Aqui você encontrará a biblioteca **beagle-grpc-android** para ajudá-lo a utilizar gRPC em um projeto usando o Beagle no Android.
Esta biblioteca conterá quase todas as configurações necessárias para adicionar gRPC em um projeto utilizando Beagle no Android.'
---

---

![Maven Central](https://img.shields.io/maven-central/v/br.com.zup.beagle/beagle-grpc?color=green&label=Beagle-gRPC)
![**Maven Central**](https://img.shields.io/maven-central/v/br.com.zup.beagle/android?label=Beagle)

### Requisitos 

Antes de começar a configurar o Beagle para o seu sistema Android, é importante verificar se você instalou todas as versões atuais dos seguintes programas:

- JDK 11+ language
- Android API level: at least 19 or higher
- Kotlin 1.5+

### Primeiros Passos

<hr>

Este tutorial configura o Beagle desde o início.

#### **Passo 1:** Adicione uma configuração ao arquivo Gradle em seu projeto:

- `Adicione mavenCentral()` no bloco `All projects` em seu arquivo do Projeto Gradle (build.gradle) <br>

```kotlin
allprojects {
    repositories {
        google()
        jcenter()
        mavenCentral()
    }
}
```

#### **Passo 2:** Adicione o plugin Kotlin Kapt e 3 dependências no gerenciador de dependências do seu aplicativo (gradle.gradle) <br>

![Maven Central](https://img.shields.io/maven-central/v/br.com.zup.beagle/beagle-scaffold?color=green&label=Beagle-Scaffold)
![Maven Central](https://img.shields.io/maven-central/v/br.com.zup.beagle/android?label=Beagle)

- Adicione o bloco de código packagingOptions no arquivo build.gradle, como no exemplo abaixo:

- _Os números de versão listados abaixo podem ter sido atualizados. Verifique as versões reais nas tags acima:_

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
}

dependencies {
    implementation "br.com.zup.beagle:android:${beagleVersion}"
    kapt "br.com.zup.beagle:android-processor:${beagleVersion}"
    implementation "br.com.zup.beagle:beagle-scaffold:1.7.1"
}
```

#### **Passo 3:** Crie um AppHttpClientFactory

- Esta classe será responsável por fornecer o `HttpClient` que será utilizado pelo Beagle para realizar as requisições.<br>

```
const val BASE_URL = "http://10.0.2.2:8080" // seu host gRPC

@BeagleComponent
class AppHttpClientFactory: HttpClientFactory {
    override fun create(): HttpClient {
        return GrpcClient(BASE_URL, HttpClientDefault())
    }
}
```

Aqui, nós estamos informando ao Beagle para utilizar `GrpcClient` para realizar requisições de rede.<br><br>
Vamos dar uma olhada nos parâmetros do contrutor de `GrpcClient`: <br>

- `grpcAddress`: Host do seu serviço gRPC. Toda requisição para este host será realizada pelo `GrpcClient`.
- `customHttpClient`: Todas as requisições feitas para outro host serão realizadas por este `HttpClient`.

Portanto, uma requisição para `http://10.0.2.2:8080/foo` será realizada pelo `GrpcClient` e uma requisição para outro host, por exemplo `http://127.0.1.1:5555/bar` será realizada pelo `HttpClientDefault`.
<br>

A implementação de `HttpClientDefault` pode ser encontrada na biblioteca Beagle-Default.

<hr>

##### Para saber mais sobre o Beagle, acesse o [repositório](https://github.com/ZupIT/beagle).

##### Para saber mais sobre o Beagle-Defaults. veja a seção do [Beagle Defaults]({{< ref path="/android/libs/beagle-defaults" lang="pt" >}}).
