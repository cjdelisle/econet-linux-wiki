---
title: EN7512 Frame Engine
description: 
published: true
date: 2025-03-20T23:26:54.322Z
tags: 
editor: markdown
dateCreated: 2025-03-20T23:25:26.867Z
---

## Frame Engine

Frame engine port numbers are listed [here](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/fe/en7512/fe_reg_en7512.h#L696).



```plantuml
@startuml FrameEngine
skinparam linetype ortho

component "Frame Engine" as Application {
    component GDM1
    component GDM2
    component PPE
    component QDMA
    
}
QDMA <-up-> (CPU)
GDM1 <-down-> (MT7530)
GDM2 <-down-> (DSL/PON)
@enduml

```

### FE_BASE

[0xBFB50000](https://github.com/cjdelisle/EN751221-Linux26/blob/master/tclinux_phoenix/modules/private/ether/en7512/eth_en7512.h#L53)



### Related Specifications
The register map does not match the MT7621 SoC but they are related.
[Chapter 2.3, Page 269](https://www.scribd.com/document/741583707/MT7621-ProgrammingGuide-DMAs-V1-1)