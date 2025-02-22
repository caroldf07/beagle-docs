---
title: Ação Customizada
weight: 2
description: Criando e executando uma ação customizada
---

---
 
**Requisitos:**
 - Ter um projeto já configurado com o Beagle.

### **Passo 1: O que é uma ação?** 

No Beagle, uma ação lida com os comportamentos (funções) que serão executadas em sua aplicação assim que um determinado evento for disparado. Essas ações podem ser padrão do próprio Beagle ou customizadas.

[**Tipos de ações do Beagle**]({{< ref path="/api/actions/overview#tipos-de-ações" lang="pt">}})


### **Passo 2: Como criar uma ação customizada:**

Para criar uma custom action, siga os seguintes passos: 

1. Crie uma struct que implemente a **interface** `Action`.

2. Depois disso, a interface solicitará que o método `execute` seja implementado.  É nesse método que deve ser implementado o bloco de código que sua ação irá executar.

```swift
struct CustomAction: Action {
    func execute(controller: BeagleController, origin: UIView) {
            print("Logica da sua ação.")
    }
}
```

Como lógica da ação vamos criar um alert. Abaixo temos o exemplo do alert.

```swift
let alert = UIAlertController(
        title: nil,
        message: "",
        preferredStyle: .alert
        )

controller.present(alert, animated: true)
```

Agora adiconamos a lógica da ação dentro do método `execute`. 
Por fim, basta criar o parametro para receber a mensagem do alert, e o init da struct.

```swift
struct CustomAction: Action {

    var mensagem: String

    init(mensagem: String? = nil) {
        self.mensagem = mensagem
    }

    func execute(controller: BeagleController, origin: UIView) {
        let alert = UIAlertController(
        title: nil,
        message: mensagem
        preferredStyle: .alert 
        )

        controller.present(alert, animated: true)
    }
}
```

### **Passo 3: Como Registrar uma ação.**

É obrigatório registrar ações no Beagle. E para fazer isso, acesse dentro do arquivo de configuração do Beagle e utilize o dependencies para registrar.

{{% alert color="info" %}} Para saber mais sobre o dependencies. [Beagle Dependencies]({{< ref path="" lang="pt" >}}). {{% /alert %}}

O método register possui dois construtores, o primeiro passando apenas o `action` e segundo recebendo o `action` e `named`.

**action:** Struct da action.

**named:** Nome da action, não obrigatório. Utiliza-se, por exemplo, quando o nome da action registrada é  diferente da criada no backend. Ele será usado na deserializações para encontrar a ação.

Veja abaixo as formas que você pode registrar:

**- A primeira forma:**
```swift
dependencies.decoder.register(action: CustomAction.self)
```

**- A segunda forma:** 
```swift 
dependencies.decoder.register(action: CustomAction.self, named: "CustomAction")
```

Após registrar o seu componente de customização, você pode usá-lo via server-driven.

### **Passo 4: Como usar um ação?**

Para usar as ações do Beagle, o componente que você for utilizar precisa de um parâmetro do tipo `Action`.

Veja abaixo um exemplo de como usar em um botão que executa a ação customizada no evento de clique:

```swift
Button(
    text: "do request",
    onPress: [
        CustomAction( mensagem: "Eu sou um Alet!")
    ]
)
```
