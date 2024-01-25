---
title: enum
date: 2024-01-25 16:19:55
tags: C++
---

```
enum {
isSpecialized = std::is_enum<T>::value, // don't require every enum to be marked manually
isPointer = false,
isIntegral = std::is_integral<T>::value,
isComplex = !qIsTrivial<T>(),
isStatic = true,
isRelocatable = qIsRelocatable<T>(),
isLarge = (sizeof(T)>sizeof(void*)),
isDummy = false, 
sizeOf = sizeof(T)
};
```
