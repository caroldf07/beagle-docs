---
title: Asynchronous Custom Action
weight: 3
description: Creating an asynchronous custom action
---

---

**Requirements:**
 -  A project set up with Beagle.
 - You need to have done the example for [**Custom Action**]({{< ref path="/ios/customization/custom-actions/custom-action" lang="en" >}}).

{{% alert color="warning" %}}
If you don't know what an action is, check the [**What is an action**]({{< ref path="/ios/customization/custom-actions/custom-action" lang="pt" >}}) section.
{{% /alert %}}

### Asynchronous custom action:

To create an asynchronous custom action in Beagle that works like an API, just create an `action`like the example in [**Custom Action**]({{< ref path="/ios/customization/custom-actions/custom-action" lang="en" >}}) and implement the `AsyncAction` interface.

After creating an action, you must implement the `AsyncAction` interface. It will request to execute a method and the onFinish parameter will be implemented.

```swift
struct CustomAction: AsyncAction {

        var onFinish: [Action]?

        func execute(controller: BeagleController, origin: UIView) {

        }
    }
```

Now you **MUST** notify when its execution is completed by triggering the `onFinish` action.

```swift
    controller.execute(actions: self.onFinish, origin: origin)
```

Follow the Asynchronous Custom Action example below:

```swift
struct CustomAction: AsyncAction {

        var onFinish: [Action]?

        func execute(controller: BeagleController, origin: UIView) {
                print("Custom action was called")

                controller.execute(actions: self.onFinish, origin: origin)
        }
    }
```

Your action is now configured to perform any tasks asynchronously!

{{% alert color="success" %}}
To learn how to register and use a new action, check the steps 3 and 4 in the [**Custom Action**]({{< ref path="/ios/customization/custom-actions/custom-action" lang="en" >}}) example.
{{% /alert %}}
