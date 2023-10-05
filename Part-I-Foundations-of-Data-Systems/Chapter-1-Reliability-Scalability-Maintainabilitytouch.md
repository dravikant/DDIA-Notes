#### Building blocks of data intensive application

- Database : store and retrieve data later
- Cache : remember the result of expensive operation
- Searching (index) : filter or search the data using key
- Stream processing : send message to another processes (handled asynchronously)
- batch processing : crunch large amount of data

No single system can cater requests of all. If at all it does, it will do it badly so use specialised systems

### Reliability 

system continue to perform the correct function at the desired level of performance

- Hardware Faults like disk /machine failure
Single machines are made more resilient through redundant hardware
Larger data and computing demands have driven move to multi-machine redundancy, often using software fault-tolerance techniques

- Software Errors like bug/process eating up resources/dependent process slow down
    - Hard to detect, can lie dormant until unusual set of circumstances
    - Can have a systematic error or cascading failures which cause multiple system failures, unlike hardware faults
    - No quick solution -- a set of small things each help:  planning, thorough testing, process isolation, allowing crash & restart, monitoring system behavior

- Human Errors like wrong input
    - Can combine several approaches to deal with human error, including:
    - Minimize opportunities for human error
    - Decouple places where people make the most mistakes from places where mistakes can cause failures(sandbox env)
    - Allow quick & easy recovery from problems
    - Use detailed and clear monitoring (telemetry/observability)
    - quick rollback

Deliberate failures (netflix chaos monkey) to test behaviour

### Scalability

as the scale increases in terms of  volume of data , traffic  or complexity of data system should handle it reasonable well


- Describing Load (writes per sec to db, cache hit rate, active users in chat room)
    Use metrics called load parameters, e.g. post tweet averages 4.6k requests/sec, peak of 12k requests/sec
    Example of Twitter’s different designs to deal with updating home timelines

    - approach 1 : join tables to retrieve the tweets (tradeoff : read is expensive)

    SELECT tweets.*, users.* FROM tweets
    JOIN users   ON tweets.sender_id    = users.id
    JOIN follows ON follows.followee_id = users.id
    WHERE follows.follower_id = current_user

    i.e. look up all the people user follow, find all the tweets for each of those users, and merge them (sorted by time)

    - appraich 2 : When a user posts a tweet, look up all the people who follow that user, and insert the new tweet into each of their home timeline caches
    (tradeoff : write is expensive)

    Fan out for celebrities is expensive

    hybrid of both approaches
    use approach 1 for celebrities and approach 2 for most others

- Describing Performance
    keeping the resources constant, with increased load -- how is the performance
        How much extra resources required to match the performance
    Common concerns are response time and throughput
    Metrics like response time are reported in percentiles, e.g. median, 95th, 99th

    The response time is what the client sees: besides the actual time to process the request (the service time), it includes network delays and queueing delays. Latency is the duration that a request is waiting to be handled—during which it is latent, awaiting service

    percentiles  used in service level objectives (SLOs) and service level agreements (SLAs), contracts that define the expected performance and availability of a service. An SLA may state that the service is considered to be up if it has a median response time of less than 200 ms and a 99th percentile under 1 s (if the response time is longer, it might as well be down), and the service may be required to be up at least 99.9% of the time. These metrics set expectations for clients of the service and allow customers to demand a refund if the SLA is not met.

    When generating load artificially in order to test the scalability of a system, the load-generating client needs to keep sending requests independently of the response time. If the client waits for the previous request to complete before sending the next one, that behavior has the effect of artificially keeping the queues shorter in the test than they would be in reality, which skews the measurements


- Approaches for Coping with Load
    Can scale up or scale out (vertical or horizontal scaling)
    Stateless services are easy to scale out, but scaling out stateful systems can introduce a lot of complexity


### Maintainability

easy to add new features/ debug / fix issues for different people working on system over period of time

use good abstractions to minimize complexity

