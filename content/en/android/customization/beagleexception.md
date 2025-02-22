---
title: BeagleException
weight: 121
description: >-
  In this section, you will find information about the BeagleException class.
---

---

## Introduction

Beagle exception is an exception class thrown when an error occurs in Beagle:

```kotlin
open class BeagleException
constructor(
    override val message: String,
    override val cause: Throwable? = null
) : Exception(message, cause)
```

| **Attributes** | **Type** | **Required** | **Description** |
| :--- | :--- | :---: | :--- |
| message | String | ✓ | Exception message thrown. |
| cause | Throwable |   | Cause of the exception. |
