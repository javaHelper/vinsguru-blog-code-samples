# Circuit Breaker Pattern With Spring Boot

Need For Resiliency:
Microservices are distributed in nature. When you work with distributed systems, always remember this number one rule – anything could happen. We might be dealing with network issues, service unavailability, application slowness etc. An issue with one system might affect another system behavior/performance. Dealing with any such unexpected failures/network issues could be difficult to solve.

Ability of the system to recover from such failures and remain functional makes the system more resilient. It also avoids any cascading failures to the downstream services.

Circuit Breaker Pattern:
In Microservice architecture, when there are multiple services (A, B, C & D), one service (A) might depend on the other service (B) which in turn might depend on C and so on. Sometimes due to some issue, Service D might not respond as expected. Service D might have thrown some exception like OutOfMemory Error or Internal Server Error. Such exceptions are cascaded to the downstream services which might result in poor user experience as shown below.



We have Retry Pattern in which we could retry couple of times until we get the proper response. The problem with this retry approach is – if it was an intermittent network issue, then it makes sense in retrying! What if it was an app issue?

Let’s consider below architecture in which Service B depends on Service C which has an issue. It is not behaving correctly. With this Retry Pattern, Service B will be sending multiple requests again and again to Service C assuming it will work. When there are hundreds of concurrent requests are sent to Service B which in turn would be bombarding Service C with Retry requests!


This is where Circuit Breaker Pattern helps us! When the requests to Service B are continuously failing, what is the point for sending the requests to Service C? Circuit Breaker simply skips the calls to Service C and goes with the fall back method / default values instead for certain duration which is configurable. That is, it does not bombard Service C continuously. It gives Service C sometime to recover from failure. Circuit Breaker Pattern retries after some time and so on.
