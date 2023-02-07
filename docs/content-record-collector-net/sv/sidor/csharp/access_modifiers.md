---
title: "Access modifiers"
date: 2023-02-07
description: "c#: access modifiers"
categories:
  - C#
images:
  - /files/net1.jpg
authorname: "Mårten Johannesson"
authorimage: "/files/mj.jpg"
robots: "noindex"
weight: 5
---

Mina noteringar runt access modifiers, eller hur man uppnår inkapsling/encapsulation.
<!--more-->
## De olika typerna av access modifiers

| Typ   | Beskrivning   |
|---|---|
| public   | tillgänglig överallt  |
| private | endast tillgänglig i samma typ - default|
| protected| tillgänglig i samma typ och de som ärver från typen |
|internal| tillgänglig i samma assembly (dvs "project" på VS-språk) |
|internal protected| tillgänglig i samma typ, i typer i samma assembly och i typ som ärver från typen. Dvs **internal**+**protected** på samma gång |
| private protected | tillgänglig i typen och typ som ärver från typen och är i samma assembly. Dvs en begränsad variant av **protected**. |


## Exempel
