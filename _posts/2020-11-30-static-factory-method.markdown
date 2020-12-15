---
layout: post
title: "Static factory method pattern illustrated with methods from the JDK"
date: 2020-11-30
categories:
  - technical 
description: Static factory method pattern illustrated with methods from the JDK
image: /assets/img/Guardrails.png
image-sm: /assets/img/Guardrails.png
---

The static factory method pattern is an object creational pattern that makes use of a static method to create an instance
of the class, in combination with (usually) a private constructor for the class. Thus, the object creation is controlled
through these factory methods.
Thus, the static factory method is different from the `Factory method` and `Abstract factory` patterns described in the
classic <a target="_blank" href="https://en.wikipedia.org/wiki/Design_Patterns">Gang of Four</a> book.

Joshua Boch in <a target="_blank" href="https://www.goodreads.com/book/show/34927404-effective-java">`Effective Java`</a> has one of the
best descriptions of this design pattern. Much of the wisdom in this article is derived from `Effective Java`.

Next, we are going to see the main highlights of the static factory methods, and examples illustrating the highlights from the JDK.

Main highlights of the static factory method:
1. They have descriptive names, improving readability.

   Consider methods such as these ones in `java.time.LocalDate`:

   ```
   // Current date from the system clock in the default time-zone
   public static LocalDate now() { ... }
   
   // Current date from the system clock in the supplied time-zone
   public static LocalDate now(ZoneId zone) { ... }
   ```
2. They can have the same signature and different names, unlike constructors.

   Two methods to illustrate this can be found in `java.util.Collections`:
   ```
   public static <T> Collection<T> synchronizedCollection(Collection<T> c)

   public static <T> Collection<T> unmodifiableCollection(Collection<? extends T> c) 
   ```
3. They can return any subtype of the return type.

   You can see this highlight in action in `java.util.Collections` in method `unmodifiableCollection`:

   ```
   // Returns a read-only version of the collection. The UnmodifableCollection is a 
   public static <T> Collection<T> unmodifiableCollection(Collection<? extends T> c) {
     return new UnmodifiableCollection<>(c);
   }
   ```
4. They don't need to return a new object each time they are invoked.

   This is nicely illustrated in `java.util.Boolean`, in method `valueOf`:
   ```
   // Returns one of the two static final instances of Boolean that correspond to the true or false primitives. 
   public static Boolean valueOf(boolean b) {
     return (b ? TRUE : FALSE);
   }
   ```

Main disadvntages:
1. Classes with non-public and non-proteted constructors cannot be subclassed.
2. The factory methods do not stand out in the documentation of the API.

As a side note, looking at the main highlights of the static factory method, I see a some similarities with smart constructors
as used in Scala.
