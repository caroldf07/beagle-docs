---
title: Parâmetros Remote View
weight: 176
description: Aprenda quais os parâmetros do Remote View e como alterá-los
---

---

## Beagle Remote View

A biblioteca do Beagle fornece um componente auxiliar para renderizar as telas server-driven, o remote view:

{{< tabs id="T81" >}}
{{% tab name="Angular" %}}
Abra o arquivo do componente que você deseja renderizar o
layout e adicione o beagle-remote-view, veja o exemplo a seguir

```text
<beagle-remote-view [loadParams]="loadParams"></beagle-remote-view>
```

{{% /tab %}}

{{% tab name="React" %}}
Abra o arquivo do componente que você deseja renderizar o
layout e adicione o BeagleRemoteView, veja o exemplo a seguir

```text
<BeagleRemoteView {...loadParams} />
```

{{% /tab %}}
{{< /tabs >}}

Tanto no Angular como no React, o remote view aceita propriedades que são chamadas de **Load Params**. É por meio dela que algumas opções são configuradas para influenciar a maneira como as telas são renderizadas.

Veja abaixo as propriedades aceitas, apenas `path` é obrigatória:

```text
import { LoadParams } from '@zup-it/beagle-web';

const params: LoadParams = {
  path: '/payload.json',
  fallback: {_beagleComponent_: 'beagle:text', text:'Fallback Error'},
  method: 'get',
  headers: {'header': 'value'},
  shouldShowLoading: true,
  shouldShowError: true,
  strategy: "network-only",
  loadingComponent: 'myComponentLoading',
  errorComponent: 'myComponentError'
}
```

| Propriedade       | Tipo                                                                                                                         | Descrição                                                                           |
| :---------------- | :--------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------- |
| path              | string                                                                                                                       | **Obrigatória.** Caminho do servidor para carregar o JSON com a tela server-driven. |
| fallback          | BeagleComponent                                                                                                              | Componente para ser renderizado caso a requisição falhe.                            |
| method            | método Http                                                                                                                  | Método Http para fazer a requisição.                                                |
| headers           | Mapa&lt;chave, valor&gt;                                                                                                     | Lista de header para anexar ao fazer a requisição.                                  |
| shouldShowLoading | booleano                                                                                                                     | Mostra ou não o componente de loading.                                              |
| shouldShowError   | booleano                                                                                                                     | Mostra ou não o componente de erro.                                                 |
| strategy          | string&lt;[**Estratégia de cache**]({{< ref path="/web/commons/cache-strategy#tipos-de-estratégias" lang="pt" >}})&gt; | Estratégia de cache adotada na requisição.                                          |
| loadingComponent  | string                                                                                                                       | Componente customizado para loading.                                                |
| errorComponent    | string                                                                                                                       | Componente customizado para error.                                                  |
