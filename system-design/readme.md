 # System design
Based on: [origin en](https://github.com/donnemartin/system-design-primer), [ru](https://github.com/voitau/system-design-primer/blob/master/README-ru.md)

## How to approach a system design interview question

> How to tackle a system design interview question.

The system design interview is an **open-ended conversation**.  You are expected to lead it.

You can use the following steps to guide the discussion.  To help solidify this process, work through the [System design interview questions with solutions](#system-design-interview-questions-with-solutions) section using the following steps.

### Step 1: Outline use cases, constraints, and assumptions

Gather requirements and scope the problem.  Ask questions to clarify use cases and constraints.  Discuss assumptions.

* Who is going to use it?
* How are they going to use it?
* How many users are there?
* What does the system do?
* What are the inputs and outputs of the system?
* How much data do we expect to handle?
* How many requests per second do we expect?
* What is the expected read to write ratio?

### Step 2: Create a high level design

Outline a high level design with all important components.

* Sketch the main components and connections
* Justify your ideas

### Step 3: Design core components

Dive into details for each core component.  For example, if you were asked to [design a url shortening service](https://github.com/donnemartin/system-design-primer/solutions/system_design/pastebin/README.md), discuss:

* Generating and storing a hash of the full url
    * [MD5](https://github.com/donnemartin/system-design-primer/solutions/system_design/pastebin/README.md) and [Base62](https://github.com/donnemartin/system-design-primer/solutions/system_design/pastebin/README.md)
    * Hash collisions
    * SQL or NoSQL
    * Database schema
* Translating a hashed url to the full url
    * Database lookup
* API and object-oriented design

### Step 4: Scale the design

Identify and address bottlenecks, given the constraints.  For example, do you need the following to address scalability issues?

* Load balancer
* Horizontal scaling
* Caching
* Database sharding

Discuss potential solutions and trade-offs.  Everything is a trade-off.  Address bottlenecks using [principles of scalable system design](#index-of-system-design-topics).

### Back-of-the-envelope calculations

You might be asked to do some estimates by hand.  Refer to the [Appendix](#appendix) for the following resources:

* [Use back of the envelope calculations](http://highscalability.com/blog/2011/1/26/google-pro-tip-use-back-of-the-envelope-calculations-to-choo.html)
* [Powers of two table](#powers-of-two-table)
* [Latency numbers every programmer should know](#latency-numbers-every-programmer-should-know)

### Source(s) and further reading

Check out the following links to get a better idea of what to expect:

* [How to ace a systems design interview](https://www.palantir.com/2011/10/how-to-rock-a-systems-design-interview/)
* [The system design interview](http://www.hiredintech.com/system-design)
* [Intro to Architecture and Systems Design Interviews](https://www.youtube.com/watch?v=ZgdS0EUmn70)
* [System design template](https://leetcode.com/discuss/career/229177/My-System-Design-Template)

## System design interview questions with solutions

> Common system design interview questions with sample discussions, code, and diagrams.
>
> Solutions linked to content in the `solutions/` folder.

| Question | |
|---|---|
| Design Pastebin.com (or Bit.ly) | [Solution](https://github.com/donnemartin/system-design-primer/solutions/system_design/pastebin/README.md) |
| Design the Twitter timeline and search (or Facebook feed and search) | [Solution](https://github.com/donnemartin/system-design-primer/solutions/system_design/twitter/README.md) |
| Design a web crawler | [Solution](https://github.com/donnemartin/system-design-primer/solutions/system_design/web_crawler/README.md) |
| Design Mint.com | [Solution](https://github.com/donnemartin/system-design-primer/solutions/system_design/mint/README.md) |
| Design the data structures for a social network | [Solution](https://github.com/donnemartin/system-design-primer/solutions/system_design/social_graph/README.md) |
| Design a key-value store for a search engine | [Solution](https://github.com/donnemartin/system-design-primer/solutions/system_design/query_cache/README.md) |
| Design Amazon's sales ranking by category feature | [Solution](https://github.com/donnemartin/system-design-primer/solutions/system_design/sales_rank/README.md) |
| Design a system that scales to millions of users on AWS | [Solution](https://github.com/donnemartin/system-design-primer/solutions/system_design/scaling_aws/README.md) |
| Add a system design question | [Contribute](#contributing) |

### Design Pastebin.com (or Bit.ly)

[View exercise and solution](https://github.com/donnemartin/system-design-primer/solutions/system_design/pastebin/README.md)

![Imgur](images/4edXG0T.png)

### Design the Twitter timeline and search (or Facebook feed and search)

[View exercise and solution](https://github.com/donnemartin/system-design-primer/tree/master/solutions/system_design/twitter/README.md)

![Imgur](images/jrUBAF7.png)

### Design a web crawler

[View exercise and solution](https://github.com/donnemartin/system-design-primer/solutions/system_design/web_crawler/README.md)

![Imgur](images/bWxPtQA.png)

### Design Mint.com

[View exercise and solution](https://github.com/donnemartin/system-design-primer/solutions/system_design/mint/README.md)

![Imgur](images/V5q57vU.png)

### Design the data structures for a social network

[View exercise and solution](https://github.com/donnemartin/system-design-primer/solutions/system_design/social_graph/README.md)

![Imgur](images/cdCv5g7.png)

### Design a key-value store for a search engine

[View exercise and solution](https://github.com/donnemartin/system-design-primer/solutions/system_design/query_cache/README.md)

![Imgur](images/4j99mhe.png)

### Design Amazon's sales ranking by category feature

[View exercise and solution](https://github.com/donnemartin/system-design-primer/solutions/system_design/sales_rank/README.md)

![Imgur](images/MzExP06.png)

### Design a system that scales to millions of users on AWS

[View exercise and solution](https://github.com/donnemartin/system-design-primer/solutions/system_design/scaling_aws/README.md)

![Imgur](images/jj3A5N8.png)



## Links:
https://github.com/donnemartin/system-design-primer