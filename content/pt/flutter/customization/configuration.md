---
title: Configuração
weight: 2
description: Nessa seção, você encontrará informações sobre como configurar o Beagle Flutter.
---

---

## Introdução
O método `BeagleSdk.init` é a porta de entrada do Beagle Flutter e o recebedor de todas as configurações que lida. Por padrão já é fornecido implementações básicas para alguns de seus parâmetros, não sendo necessário configurar todos eles. 

## O que é configurável?
Essas são todas as opções de configuração que o Beagle Flutter suporta:

1. environment
2. baseUrl
3. httpClient
4. components
5. storage
6. useBeagleHeaders
7. actions
8. strategy
9. navigationControllers
10. designSystem
11. imageDownloader
12. logger
13. operations

Confira abaixo como configurar cada um:

### 1. environment
Esse atributo informa ao Beagle sobre o atual estado de build da aplicação. Esta propriedade é usada no `BeagleUndefinedWidget`.
- Ele mostra um Text com a descrição `Undefined Component` quando o `environment` é `BeagleEnvironment.debug`.
- Ele mostra um `SizedBox.shrink` quando `BeagleEnvironment.production`.

O padrão é `BeagleEnvironment.debug`.
```dart
enum BeagleEnvironment {
  debug,
  production,
}
```

### 2. baseUrl
Informa a URL base usada no Beagle na aplicação.

As requisições feitas pelo `BeagleWidget` e ações de navegação usam a propriedade `baseUrl` para montar seus paths. O padrão é uma string vazia. No exemplo abaixo, a request resultante vai ser `https://usebeagle.io/start/welcome`:

```dart
BeagleSdk.init(
  baseUrl: 'https://usebeagle.io/start',
);

BeagleWidget(
  screenRequest: BeagleScreenRequest('welcome'),
);
```

### 3. httpClient
Uma interface que provê um cliente para o Beagle fazer as requisições.

O padrão é o `DefaultHttpClient`. Este é um componente chave para o Beagle, porque ele é usado em todas as requisições. Veja abaixo o contrato `HttpClient`:

```dart
abstract class HttpClient {
  Future<Response> sendRequest(BeagleRequest req);
}
```

### 4. components
Provê uma estrutura onde a chave é a propriedade `_beagleComponent_` no JSON e o valor é o widget que o Beagle deve renderizar.

Se você quiser usar uma lista de widgets comuns com implementação básica:
- Use o pacote `beagle_components` no seu projeto.

O padrão é um mapa vazio. Aqui está um exemplo da declaração de componente:
```dart
Map<String, ComponentBuilder> myComponent = {
  'custom:loading': (element, _, __) {
    return Center(
      key: element.getKey(),
      child: const Text('My Component'),
    );
  }
};
```

### 5. storage
Cuida do armazenamento de cache para as requisições feitas pelo Beagle. O padrão é o `DefaultStorage`.

Para evitar problemas de vulnerabilidade, a implementação padrão armazena a informação somente em memória, veja abaixo:
```dart
class DefaultStorage implements Storage {
  Map<String, String> storage = {};

  @override
  Future<void> clear() async {
    storage.clear();
  }

  @override
  Future<String> getItem(String key) async {
    return storage[key];
  }

  @override
  Future<void> removeItem(String key) async {
    storage.remove(key);
  }

  @override
  Future<void> setItem(String key, String value) async {
    storage[key] = value;
  }
}
```

### 6. useBeagleHeaders
Controla se deve ou não enviar cabeçalhos específicos do Beagle em solicitações para buscar um widget. O padrão é `true`.

### 7. actions
Este é o mapa de ações customizadas. A chave deve ser o identificador `_beagleAction_` no JSON e o valor deve ser o action handler. O padrão é o mapa `defaultActions`. Veja abaixo uma declaração desta propriedade:
```dart
Map<String, ActionHandler> myAction = {
  'custom:log': ({action, _, __, ___}) {
    debugPrint(action.getAttributeValue('message'));
  }
};
```

### 8. strategy
Essa propriedade diz ao Beagle como cuidar do cache das requisições. Existem sete eestratégias de cache que o Beagle implementa. O padrão é `BeagleNetworkStrategy.beagleWithFallbackToCache`. Confira abaixo todas elas:
```dart
enum BeagleNetworkStrategy {
  beagleCacheOnly,
  beagleWithFallbackToCache,
  networkWithFallbackToCache,
  cacheWithFallbackToNetwork,
  cacheOnly,
  networkOnly,
  cacheFirst,
}
```

### 9. navigationControllers
Estas são as opções para feedback visual quando navegando de um widget para outro.

Para atribuir as opções padrões:
- Use `isDefault: true` no controlador de navegação.

O padrão é um mapa vazio. Confira abaixo uma declaração de um `NavigationController` customizado:
```dart
Map<String, NavigationController> myController = {
  'general': NavigationController(
    isDefault: true,
    loadingComponent: 'custom:loading',
  ),
};
```

### 10. designSystem
Uma interface que provê um design do sistema para os componentes do Beagle. Ele habilita imagens e estilos de botões e textos no Beagle. O padrão é o `DefaultEmptyDesignSystem`, que não retorna nenhum valor. Veja abaixo o contrato `BeagleDesignSystem`:
```dart
abstract class BeagleDesignSystem {
  String image(String id);

  BeagleButtonStyle buttonStyle(String id);

  TextStyle textStyle(String id);
}
```

### 11. imageDownloader
Uma interface que provê imagens da internet. Ele deve implementar o methodo `downloadImage` que recebe a URL da imagem e retorna um `Future<Uint8List>` como imagem. O padrão é o `DefaultBeagleImageDownloader`.
```dart
class DefaultBeagleImageDownloader implements BeagleImageDownloader {
  DefaultBeagleImageDownloader({
    @required this.httpClient,
  }) : assert(httpClient != null);

  final HttpClient httpClient;

  @override
  Future<Uint8List> downloadImage(String url) async {
    final request = BeagleRequest(url);
    final response = await httpClient.sendRequest(request);

    if (response.status != HttpStatus.ok) {
      throw BeagleImageDownloaderException(
          statusCode: response.status, url: request.url);
    }

    final bytes = response.bodyBytes;
    if (bytes.lengthInBytes == 0) {
      throw Exception('Image is an empty file: $url');
    }

    return bytes;
  }
}
```

### 12. logger
Interface que provê o logger para o Beagle utilizar na aplicação. O padrão é o `DefaultEmptyLogger` que não loga nenhuma informação. Veja o contrato `BeagleLogger`:
```dart
abstract class BeagleLogger {
  void warning(String message);

  void error(String message);

  void errorWithException(String message, Exception exception);

  void info(String message);
}
```

### 13. operations
Um mapa de operações customizadas que podem ser usadas para extender a capacidade das expressões do Beagle e são chamadas como funções, por exemplo `@{sum(1, 2)}`. O padrão é um mapa vazio.
