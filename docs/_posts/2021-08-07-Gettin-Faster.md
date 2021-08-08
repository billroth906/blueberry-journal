---
layout: post
title: "Gettin' Faster"
date: 2021-08-07 23:00:00 -0400
categories: sdc
---
### Express Perplexion
I ran into a quandry today that I haven't been able to Google my way out of. The Hack Reactor API for Project Atelier sent some ids back in JSON as numbers. But utilizing Express, both .json and .send throw the JSON through a full round of JSON.stringify before rolling them through the response. From the rhetoric online, this seems to be the desired way to move JSON. The weird thing is that there doesn't seem to be any way around it utilizing Express if the header is to stay 'application/JSON.' So I'm still not sure about how I am going to resolve this one.

### Indexing Really Did Change Mostly Everything
It is amazing how much of a difference Postgres indexing of the product_id and style_id columns did for response times. That change alone brought most of my API responses to under 50ms. The other optimization I made was through cleaning up the logic. For 3 of the 4 calls, I'm only calling the database at once, rather than stringing calls in promises. For styles I found that it still made sense for two steps, being that both I don't have product ids in photos and skus, and that the data in photo and skus being returned is well nested. All APIs are now responding locally in the 7ms to 15ms range.

[API Response Times](https://drive.google.com/file/d/1VHmgM7S9w9HpHDEAzNsbpJMg0AKdDvq3/view?usp=sharing)

Stress testing has also made a dramatic turn. One of the problems that I noticed through stress testing was that styles were failing completely when there were not any pictures or skus attached to the styles being requested. That was affecting about 20% of the styles queries after the response time optimizations were made. Now all 100% of queries are returning 200. Individual Products went from handling 6 requests per second to 213. Products went from handling 213 request per second to 243. Related went from handling 3 requests per second to 259. Styles went from handling 0.5 requests per second to 131.

[Product Stress Test](https://drive.google.com/file/d/1WAYqr4gjbkT3vWKA-drc7WffeBY8a4_W/view?usp=sharing)

[Products Stress Test](https://drive.google.com/file/d/1igIMDxype2EYW_hpsTiWBGLwluBTI7OE/view?usp=sharing)

[Related Stress Test](https://drive.google.com/file/d/1xfXK-nI8s_smX3721Va4Oxb9z_1Jxq_Y/view?usp=sharing)

[Styles Stress Test](https://drive.google.com/file/d/1iEsntz0hLEJqQdgLSdRxBkHrQpoPfXTv/view?usp=sharing)
