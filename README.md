# PURPOSE:  TO REDUCE LATENCY IN URL SHORTENER

September 29, 2023

By:  Annie V Lam - Kura Labs

# Background

Nike contacted us regarding our URL Shortener. In their email, Nike mentioned customer complaints about slow webpage loading times and has requested that we work on reducing latency for their customers.  

## Troubleshooting

For testing purposes, to avoid impacting the production web server, we created a separate web server in the same region and VPC but in different availability zones (AZs) from the production environment. This setup ensures comparability while preventing disruptions.

We utilized JMeter, a testing tool, to measure average latency. We configured it to send 1000 GET requests within 2 seconds to assess the entire response time, from request initiation to content delivery. The results indicate that our URL Shortener website currently has an average response time (latency) of 40.879 ms, which exceeds our desired target  

![Latency](Images/Web_Server_Latency.png)

## Possible Resolution

Incorporating a Content Delivery Network (CDN) is expected to reduce latency significantly by caching web content. Since our entire infrastructure is hosted on the AWS platform, we opted to implement AWS's CDN service, CloudFront. After integrating CloudFront's CDN distribution, we conducted latency tests using JMeter with the same parameters—1000 GET requests within 2 seconds. The results showed a remarkable reduction in latency to 9.652 ms.

![Latency](Images/CDN_Latency.png)

## Conclusion

Integrating a CDN, which utilizes a network of interconnected servers and caching, significantly improved the delivery speed of our web content and reduced latency.  In our case with the URL Shortener, latency went from 41ms to 10ms, which is a 75% reduction in latency.  As part of this enhancement, we also integrated AWS CloudFront into our production web server architecture.  

![Diagram](Images/Blitz_1_Diagram.png)


