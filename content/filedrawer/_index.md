---
date: "2021-08-24"
diagram: true
linkTitle: My File Drawer
summary: Page summary for search engines.
title: "My File Drawer"
type: book
draft: true
---

{{< figure src="featured.jpg" >}}



{{< toc hide_on="xl" >}}

Hi, and welcome to My File Drawer! Here, you will find all 13 experiments I ran through my Stanford Qualtrics account.

The phrase file-drawer problem was first coined by a distinguished psychologist several decades ago (Rosenthal, 1979)

Here's a quick overview of how I got to 13 experiments in total:


```mermaid
graph LR
A[61 experiments <br> in Qualtrics account] 
A ==>|included|B[31 created by me]
A -.->|excluded|C[30 shared by others]
B ==>|included|D[19 experiments <br> with data]
B -.->|excluded|E[12 drafts without <br> data collection]
D ==>|included|F[13 file-drawer experiments]
D -.->|excluded|G[6 belonging to projects <br> published or under revision]
style A fill:#f6d4d1,stroke:#333,stroke-width:4px
style B fill:#f6d4d1,stroke:#333,stroke-width:4px
style D fill:#f6d4d1,stroke:#333,stroke-width:4px
style F fill:#f6d4d1,stroke:#333,stroke-width:4px
style C fill:#f7f7f7,stroke:#333,stroke-width:4px
style E fill:#f7f7f7,stroke:#333,stroke-width:4px
style G fill:#f7f7f7,stroke:#333,stroke-width:4px
```

## File drawer overview

{{< list_children >}}


{{< cta cta_text="Continue to my file drawer" cta_link="facial" >}}
