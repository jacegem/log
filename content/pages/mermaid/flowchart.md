---
tags:
- mermaid
- flowchart
title: mermaid/flowchart
categories:
date: 2025-01-18
lastMod: 2025-01-18
---




_markup/render 만 활성화











```mermaid
erDiagram
CUSTOMER ||--o{ ORDER : places
ORDER ||--|{ LINE-ITEM : contains
CUSTOMER }|..|{ DELIVERY-ADDRESS : uses
```







```mermaid
erDiagram
CUSTOMER ||--o{ ORDER : places
ORDER ||--|{ LINE-ITEM : contains
CUSTOMER }|..|{ DELIVERY-ADDRESS : uses
```









```mermaid
graph TD;
A-->B;
A-->C;
B-->D;
C-->D;
```



mermaid mardown?

```mermaid {class="mermaid"}
flowchart LR
    id
```



shell?

```shell
flowchart LR
    id
```
