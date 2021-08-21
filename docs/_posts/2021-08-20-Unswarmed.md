---
layout: post
title: "Moving to Unswarmed"
date: 2021-08-20 23:00:00 -0400
categories: sdc
---
### The First Views from Loader.io
Preliminary results from Loader.io on the deployed Docker swarm left much to be desired. With just a few requests per second, the outcomes were not too bad: response from Products at 6ms, (single) Product at 17ms, Styles at 20ms, and Related at 9ms. But after notching the requests up to a mere 1000 requests per minute (or about 17 requests per second), the deployed swarm began to reach its limit, yielding response times ranging from about 3 to 8 seconds. The Styles API was not able to go above 750 requests per minute, with average responses at that rate yielding 7683ms.

The Highest I could crank Loader.io:

[Products Stress Test](https://drive.google.com/file/d/1l7KLM4xPv0ocfiWqd22L0mEyS3YUD6zg/view?usp=sharing)

[(Single) Product Stress Test](https://drive.google.com/file/d/1LOlnLElx3T-y3x-csh0T5t7UCCtf4-jZ/view?usp=sharing)

[Related Stress Test](https://drive.google.com/file/d/1stOp0j4nwhikU__TQcfRdyf_gsKppd9T/view?usp=sharing)

[Styles Stress Test](https://drive.google.com/file/d/14LezVDFQkDu8XB5qu8YRpl1Dqh14xG9G/view?usp=sharing)

### Road to Improvement Brought Disaster
The first inclination that I had to improve the performance of the APIs was to add one or more additional servers to handle the traffic along with a load balancer to orchestrate them. Either after having added an additional server (or perhaps even slightly prior), it became evident that I had a Gluster brick that was corrupted. Though in theory all Gluster bricks replicate each other, and therefore, again theoretically, no data was lost, it becomes seemingly impossible to mount new Gluster bricks unless all of the rest are fully online. I did not find a reasonable way to rectify this issue in a fashion that would have had me not going into very far flung territory. So I made the decision to unswarm my Docker containers, and rebuild them one per instance. In that process I added an NGINX load balancer to the arrangement as well.

### Results from Standard Docker Containers with Load Balancer
I had expected the extra server, the load balancer, and the more direct network traffic (without having to find the righ machine on the Docker swarm) to improve the previous results dramatically. In all cases, the APIs were able to handle about 300 more requests per minute, and unstressed response times were only about 2/3 of their previous numbers. But error rates are still prohibiting dependable retults over 1000 requests per minute with over 1/2 the cases failing over that rate. For doubling the server capacity, there is not double the successful responses. PM2 is identifying CPU usage rise to around 100% when about 1000 requests per minute are sent to any of the routes. This may be a hint that even more server capacity could be useful.

Breaking Point Loader Results:

[Products Stress Test](https://drive.google.com/file/d/1-BbOVHQ1KxHwgCMUdFI-4tsFKmRRBPyL/view?usp=sharing)

[(Single) Product Stress Test](https://drive.google.com/file/d/1Ov5dQJd1Wyi_fHM5IPemb4uJTLh9LHGv/view?usp=sharing)

[Related Stress Test](https://drive.google.com/file/d/1FaBW-aIp5f4x7pom84biRwwtzJokxtsy/view?usp=sharing)

[Styles Stress Test](https://drive.google.com/file/d/10kKMOCv0awYWqfJkNcRar1JLDZi9kOMt/view?usp=sharing)
