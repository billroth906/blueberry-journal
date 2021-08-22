---
layout: post
title: "Improved But Almost the Same"
date: 2021-08-21 23:00:00 -0400
categories: sdc
---
### Glass Ceiling
There are many that have sung the praises of throwing Redis as a cache in front of Postgres. But the music hasn't played out here yet. Upon inspecting Redis through redis-cli, it does indeed appear that keys are being successfully cached. In addition, two Express servers have been added to the original two. But still, as before, once thenumber of clients per second surpasses 1034, response times rise hundredsfold and timeouts appear. This is also despite the fact that nginx worker connections are set at 4096 (up from the default of 1024).

Loader.io results, still resemblant of a 1034 ceiling:

[Products Stress Test](https://drive.google.com/file/d/1IY3lSB8A63u9-t_MqBkgi4oKiqMGvtGb/view?usp=sharing)

[(Single) Product Stress Test](https://drive.google.com/file/d/1VBxXylECS3Ju8pvy-N3eRZGNYzVYWDOv/view?usp=sharing)

[Related Stress Test](https://drive.google.com/file/d/1l_aQ0bPbfvRiRDS0WshRHZ8IhGnQ-JOI/view?usp=sharing)

[Styles Stress Test](https://drive.google.com/file/d/1_JAlDDHUl6MrNMELUByZmorLkvCpdhdd/view?usp=sharing)