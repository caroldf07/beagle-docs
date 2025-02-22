---
title: ResetStack
weight: 90
description: Here you'll find ResetStack description and its attribute.
---

---

## What is it?
What is it?
Opens a screen with an informed route of a new flow and cleans the pile of previous loaded screens.

Your structure is represented by the attribute below:

| **Attribute** | Type                                           | Required | Definition        |
| :------------ | :--------------------------------------------- | :------- | :---------------- |
| route         | [**Route**]({{< ref path="/api/actions/navigate/route/" lang="en" >}}) | ✓        | Navigation route. |
| controllerId | String |     | The navigation controller id to be used during the navigation action, If missing, the default navigation controller will be used instead. |

## How to use it?

On the example below, three screens were used. The two first used PushView to add the screens to the piles and the last one uses **ResetStack** and reopens the first screen.

You will need three endpoints to test:

1. The first endpoint will be what your frontend will call to render the screen zero.
2. The second endpoint must be mapped **"/firstScreen"**, because this will be the chosen URL of the screen 0 button's navigation and for that the endpoint must return the screen 1.
3. The third point must be mapped**"/secondScreen",** because this will be the chosen URL of the screen 1 button's navigation and for that this endpoint must return the screen 2. On the screen 2, the passed route must be the screen endpoint that you want to return when the application restarts. In this case, it is **"/home"** that it is screen's 0 endpoint.

#### How to call the screen zero <a id="como-chamar-a-tela-zero"></a>

{{< tabs id="T101" >}}
{{% tab name="JSON" %}}

<!-- json-playground:firstScreenonResetStack.json
{
  "_beagleComponent_" : "beagle:screenComponent",
  "child" : {
    "_beagleComponent_" : "beagle:container",
    "children" : [ {
      "_beagleComponent_" : "beagle:text",
      "text" : "First Screen on ResetStack"
    }, {
      "_beagleComponent_" : "beagle:button",
      "text" : "Click me!",
      "onPress" : [ {
        "_beagleAction_" : "beagle:pushView",
        "route" : {
          "url" : "SecondScreenonResetStack.json",
          "shouldPrefetch" : false
        }
      } ]
    } ]
  }
}
-->

{{% playground file="firstScreenonResetStack.json" language="en" %}}
{{% /tab %}}

{{% tab name="Kotlin DSL" %}}

```kotlin
Screen(
    child = Container(
        children = listOf(
            Text(
                "First Screen on Stack"
            ),
            Button(
                text = "Click me!",
                onPress = listOf(
                    Navigate.PushView(
                        Route.Remote(
                            url = "SecondScreenonResetStack.json"
                        )
                    )
                )
            )
        )
    )
)
```

{{% /tab %}}
{{< /tabs >}}

#### How to call the screen 1 <a id="como-chamar-a-tela-1"></a>

{{< tabs id="T102" >}}
{{% tab name="JSON" %}}

<!-- json-playground:SecondScreenonResetStack.json
{
  "_beagleComponent_" : "beagle:screenComponent",
  "child" : {
    "_beagleComponent_" : "beagle:container",
    "children" : [ {
      "_beagleComponent_" : "beagle:text",
      "text" : "Second Screen on Stack"
    }, {
      "_beagleComponent_" : "beagle:button",
      "text" : "Click me!",
      "onPress" : [ {
        "_beagleAction_" : "beagle:pushView",
        "route" : {
          "url" : "resetStack.json",
          "shouldPrefetch" : false
        }
      } ]
    } ]
  }
}
-->

{{% playground file="SecondScreenonResetStack.json" language="en" %}}
{{% /tab %}}

{{% tab name="Kotlin DSL" %}}

```kotlin
Screen(
    child = Container(
        children = listOf(
            Text(
                "Second Screen on Stack"
            ),
            Button(
                text = "Click me!",
                onPress = listOf(
                    Navigate.PushView(
                        Route.Remote(
                            url = "resetStack.json"
                        )
                    )
                )
            )
        )
    )
)
```

{{% /tab %}}
{{< /tabs >}}

#### How to call the screen 2 <a id="como-chamar-a-tela-2"></a>

{{< tabs id="T103" >}}
{{% tab name="JSON" %}}

<!-- json-playground:resetStack.json
{
  "_beagleComponent_" : "beagle:screenComponent",
  "child" : {
    "_beagleComponent_" : "beagle:container",
    "children" : [ {
      "_beagleComponent_" : "beagle:text",
      "text" : "Third Screen on Stack"
    }, {
      "_beagleComponent_" : "beagle:button",
      "text" : "Click me to go to reset stack",
      "onPress" : [ {
        "_beagleAction_" : "beagle:resetStack",
        "route" : {
          "url" : "firstScreenonResetStack.json",
          "shouldPrefetch" : false
        }
      } ]
    } ]
  }
}
-->

{{% playground file="resetStack.json" language="en" %}}
{{% /tab %}}

{{% tab name="Kotlin DSL" %}}

```kotlin
Screen(
    child = Container(
        children = listOf(
            Text(
                "Third Screen on Stack"
            ),
            Button(
                text = "Click me  to reset stack",
                onPress = listOf(
                    Navigate.ResetStack(
                        route = Route.Remote("firstScreenonResetStack.json")
                    )
                )
            )
        )
    )
)
```

{{% /tab %}}
{{< /tabs >}}
