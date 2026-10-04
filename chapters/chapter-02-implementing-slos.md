# Chapter 2: Implementing Service Level Objectives 

## Service Level Objectives 

A **Service Level Objective (SLO)** defines the target level of reliability expected from a service. 

SLOs help teams: 

- Make data-driven reliability decisions 
- Prioritize engineering work 
- Balance reliability with feature development 
- Create clarity during decision-making 

--- 

## Getting Started with SLOs 

Before implementing an SLO: 

1. Get agreement from the relevant product stakeholders. 
2. Select an SLO that is achievable under normal operating conditions. 
3. Use the error budget to prioritize engineering work. 
4. Establish a process for reviewing and refining the SLO. 

An SLO should guide real engineering and business decisions rather than exist only as a monitoring metric. 

--- 

## Error Budgets 

A target of 100% reliability is generally unsuitable because: 

- Hardware and software components can fail. 
- Users can make unexpected requests. 
- Deployments and configuration changes can introduce failures. 
- Pursuing perfection can restrict innovation. 
- The cost of achieving higher reliability may outweigh its value. 

Instead of targeting 100%, teams establish a realistic SLO and calculate the corresponding **error budget**. 

### Error Budget Formula 

```text 
Error Budget = 100% - SLO 
``` 

### Example 

Suppose a service has: 

```text 
SLO = 99.9% 
``` 

The error budget is: 

```text 
Error Budget = 100% - 99.9% 
 = 0.1% 
``` 

For 3,000,000 requests: 

```text 
Allowed failed requests = 3,000,000 × 0.001 
 = 3,000 
``` 

Therefore, the service can experience up to **3,000 failed requests** while remaining within its error budget. 

### Understanding Reliability Values 

- **0% success:** Nothing works. 
- **100% success:** Nothing fails. 
- **SLO below 100%:** A controlled amount of failure is acceptable. 

Once an SLO is defined, someone must take ownership of implementing it, monitoring it, and improving service reliability. 

--- 

## What to Measure 

### Service Level Indicator 

A **Service Level Indicator (SLI)** is a quantitative measurement of the level of service provided to users. 

An SLI is generally represented as the ratio of good events to total valid events: 

```text 
SLI = Good Events / Total Valid Events 
``` 

### Examples 

#### HTTP success rate 

```text 
Successful HTTP Requests / Total HTTP Requests 
``` 

#### gRPC latency 

```text 
gRPC Calls Completed in Less Than 100 ms / Total gRPC Calls 
``` 

#### User experience 

```text 
Good User Minutes / Total User Minutes 
``` 

If the ratio is: 

- **0%**, the service is not providing successful outcomes. 
- **100%**, every measured event satisfies the defined condition. 

--- 

## SLI Specification and Implementation 

### SLI Specification 

An **SLI specification** defines the service outcome that matters to users, independently of how the outcome is measured. 

For example: 

> The proportion of user requests completed successfully within the expected response time. 

### SLI Implementation 

An **SLI implementation** defines how the selected outcome will be measured in practice. 

Possible measurement sources include: 

- NGINX or Apache server logs 
- Browser probes 
- Page-load measurements 
- JavaScript instrumentation 
- Window performance data 

A single SLI specification may have multiple implementations. Each implementation can have trade-offs involving: 

- Accuracy 
- Coverage 
- Cost 
- Complexity 

No implementation will be perfect. Teams should continuously refine SLIs by using feedback from production systems. 

An SLI should not be selected only because current monitoring data already exists. Beginning exclusively with available measurements can lead to unnecessary or overly strict SLOs. 

--- 

## Common SLI Categories 

### 1. Availability 

Availability measures whether the service is accessible and successfully performs the requested operation. 

```text 
Successful Requests / Total Valid Requests 
``` 

--- 

### 2. Latency 

Latency measures how quickly users receive a response. 

```text 
Requests Completed Below the Latency Threshold / Total Valid Requests 
``` 

Example: 

```text 
Requests Completed in Less Than 100 ms / Total Valid Requests 
``` 

--- 

### 3. Freshness 

Freshness measures whether data has been updated within an acceptable time. 

Example: 

```text 
Inventory Data Updated Within 10 Minutes / Total Inventory Updates 
``` 

--- 

### 4. Durability 

Durability measures whether stored data remains protected from loss. 

A durability SLI may track the proportion of data that remains available and intact over a defined measurement period. 

--- 

### 5. Correctness 

Correctness measures whether the system returns accurate results. 

Example questions include: 

- Is the returned data correct? 
- Did the service perform the requested operation correctly? 

--- 

### 6. Quality 

Quality measures whether the delivered data or service result meets the expected standard and remains relevant to the user. 

--- 

### 7. Coverage 

Coverage measures how much of the relevant user activity, service traffic, or data is included in the measurement. 

A high-quality SLI should represent the actual user experience rather than only a small or convenient subset of system activity. 

--- 

## Creating the First SLI 

A practical approach to defining an initial SLI is: 

### Step 1: Choose an Application 

Select one application or service to evaluate. 

### Step 2: Identify the Users 

Determine who uses the application and which users depend on it. 

### Step 3: List Common User Activities 

Identify the important operations users perform. 

Examples include: 

- Signing in 
- Searching for information 
- Submitting a request 
- Loading a page 
- Completing a transaction 

### Step 4: Draw the Architecture 

Create a basic architecture diagram showing the components involved in delivering the user journey. 

This helps identify: 

- Dependencies 
- Request paths 
- Measurement points 
- Potential failure points 

### Step 5: Select a Simple, Measurable SLI 

Choose an indicator that: 

- Represents a meaningful user outcome 
- Can be measured consistently 
- Is understandable to stakeholders 
- Supports practical decision-making 

Prefer a simple and useful indicator over a complicated measurement that is difficult to maintain. 

--- 
--- 

## Types of Service Components 

To define meaningful SLIs, first identify the type of component being measured. A system can usually be divided into several common component types. 

### 1. Request-Driven Components 

A request-driven component receives a request and returns a response. 

Examples include: 

- Web servers 
- APIs 
- Microservices 
- Authentication services 

For request-driven components, useful SLIs include: 

- Availability 
- Latency 
- Correctness 
- Response quality 

Example availability SLI: 

```text 
Successful Requests / Total Valid Requests 
``` 

Example latency SLI: 

```text 
Requests Completed Within 100 ms / Total Valid Requests 
``` 

--- 

### 2. Pipeline Components 

A pipeline component receives an input, processes a sequence of events, and produces an output. 

Examples include: 

- Data-processing pipelines 
- Event-streaming systems 
- ETL pipelines 
- Message-processing systems 

Useful SLIs for pipeline components include: 

- Freshness 
- Coverage 
- Correctness 
- Processing latency 

For example, a freshness SLI can measure whether pipeline output is produced within an acceptable period. 

```text 
Outputs Produced Within the Time Limit / Total Expected Outputs 
``` 

--- 

### 3. Storage Components 

A storage component accepts data and stores it for future retrieval. 

Examples include: 

- Databases 
- Object storage 
- File systems 
- Backup systems 

Useful SLIs for storage components include: 

- Durability 
- Availability 
- Correctness 
- Retrieval latency 

Durability measures whether stored data remains protected from loss. 

```text 
Data Successfully Retained / Total Data Stored 
``` 

--- 

## SLI Specification vs. SLI Implementation 

An important distinction must be made between: 

- **What should be measured** 
- **How it should be measured** 

### SLI Specification: What to Measure 

An SLI specification describes the service outcome that matters to users without depending on a specific monitoring tool or data source. 

Examples include: 

- For availability, measure the proportion of successful requests. 
- For latency, measure how long the service takes to respond. 
- For freshness, measure whether data has been updated within the expected period. 
- For durability, measure whether stored data remains safe from loss. 

A specification should represent a meaningful and observable user experience. 

--- 

### SLI Implementation: How to Measure It 

An SLI implementation defines the actual method and data source used to collect the measurement. 

Different implementations may measure the same SLI specification. 

### Possible Measurement Sources 

#### Application and Server Logs 

Measurements can be collected from: 

- Application source logs 
- NGINX logs 
- Apache logs 
- Spring Boot logs 
- FastAPI logs 

Server-side logs can help measure request success, failures, response codes, and latency. 

#### Load Balancer Monitoring 

Measurements can also be collected from: 

- AWS Application Load Balancer 
- NGINX Ingress 
- Google Cloud Load Balancing 

Load balancer data can provide information about traffic, request status, and response times. 

#### Black-Box Monitoring 

A black-box monitoring system sends requests to a service and observes the response like a real user. 

It verifies the service externally without depending on its internal implementation. 

Black-box monitoring can help measure: 

- Availability 
- Response latency 
- End-to-end service behaviour 

#### Client-Side Instrumentation 

Client-side measurements can be collected using: 

- Browser logs 
- JavaScript instrumentation 
- Page-load measurements 
- Browser performance APIs 

Client-side instrumentation can provide a measurement closer to the actual user experience. 

--- 

## Selecting an SLI Implementation 

Start with the simplest useful implementation supported by the available data. 

For example, if relevant NGINX server logs are already available, those logs can be used as an initial source for measuring request availability or latency. 

However, teams must verify that the available data is: 

- Relevant to the user experience 
- Sufficient for the selected SLI 
- Accurate enough for decision-making 
- Representative of important service traffic 

In addition to availability and latency, the implementation may need to consider: 

- Freshness 
- Coverage 
- Correctness 
- Quality 
- Durability 

A single SLI specification may have multiple possible implementations. Each implementation may have advantages and disadvantages concerning: 

- Accuracy 
- Cost 
- Complexity 
- Measurement coverage 
- Proximity to the actual user experience 

SLI implementations should improve continuously through feedback from production systems. 

--- 

## Choosing an SLO Time Window 

An SLO must be evaluated over a defined period known as the **SLO time window**. 

The selected window affects how teams interpret reliability and consume the error budget. 

### Rolling Window 

A rolling window continuously measures performance over the most recent defined period. 

For example, a 30-day rolling window always considers the latest 30 days. 

Rolling windows are generally closer to the current user experience because older events automatically leave the measurement window as new events enter it. 

### Calendar Window 

A calendar window measures performance during a fixed period. 

Examples include: 

- A calendar week 
- A calendar month 
- A business quarter 

Calendar windows are useful for: 

- Business planning 
- Periodic reporting 
- Comparing fixed reporting periods 

### Short Time Windows 

A short window, such as one week, can support quick operational decisions. 

Benefits include: 

- Faster feedback 
- Quicker detection of reliability problems 
- Greater responsiveness to recent incidents 

However, short windows may be more sensitive to temporary changes or individual incidents. 

### Long Time Windows 

Longer windows are more useful for strategic decisions and long-term reliability trends. 

Benefits include: 

- More stable measurements 
- Better visibility into long-term service behaviour 
- Support for strategic planning 

However, recent reliability problems may have a smaller immediate effect on the overall result. 

The selected window should support both the service’s operational needs and its business objectives. 

--- 

## Stakeholder Agreement for SLOs 

An SLO is effective only when the relevant stakeholders agree on its target, implementation, and consequences. 

### Product Manager 

The product manager should agree that the selected SLO represents an acceptable user experience and supports the product’s business requirements. 

### Development Team 

The development team should agree to take the necessary corrective actions when the error budget is exhausted or consumed too quickly. 

These actions may include: 

- Prioritizing reliability fixes 
- Reducing risky changes 
- Improving testing 
- Addressing known system weaknesses 

### SRE Team 

The SRE team should verify that: 

- The SLO is realistic. 
- The SLI can be measured consistently. 
- The reliability target can be achieved without excessive toil. 
- Supporting the SLO will not cause unsustainable operational burden or burnout. 

Stakeholder agreement ensures that the SLO influences real decisions instead of being treated as only a dashboard metric. 

--- 

## Error Budget Policy 

An **error budget policy** defines the actions that teams should take when the service consumes or exhausts its error budget. 

The policy creates an agreed response to reliability problems and reduces confusion during incidents. 

### Common Actions 

#### Prioritize Reliability Bugs 

Engineering teams may give reliability-related problems priority over other planned work. 

Examples include: 

- Fixing recurring failures 
- Addressing performance bottlenecks 
- Improving monitoring 
- Removing dangerous manual processes 
- Strengthening automated recovery 

#### Pause Feature Development 

Feature development may be temporarily reduced or paused so engineering capacity can be redirected toward service reliability. 

The goal is not to punish the development team. The goal is to restore the service to an acceptable reliability level. 

#### Apply a Production Freeze 

A temporary production freeze may be introduced to reduce additional risk. 

During the freeze, only essential or reliability-related changes should be considered for deployment. 

A production freeze helps prevent additional instability while teams investigate and resolve existing reliability problems. 

--- 

## Documenting the SLO and Error Budget Policy 

SLO documentation should provide enough information for stakeholders to understand: 

- What is being measured 
- Why it is being measured 
- How it is being measured 
- What reliability target has been selected 
- What happens when the error budget is exhausted 

The documentation should include the following sections. 

### 1. Service Description 

Describe: 

- The purpose of the service 
- The users of the service 
- The important user journeys 
- The service’s major dependencies 

### 2. SLO Objectives 

Record the selected reliability target and the expected user outcome. 

Example: 

```text 
99.9% of valid user requests should complete successfully 
over the selected measurement window. 
``` 

### 3. SLI Implementation 

Document: 

- The SLI specification 
- The measurement formula 
- The data source 
- Any filters or exclusions 
- The selected measurement window 

Example: 

```text 
SLI = Successful Valid Requests / Total Valid Requests 
``` 

### 4. Error Budget Calculation 

Document how the error budget is calculated. 

```text 
Error Budget = 100% - SLO 
``` 

Example: 

```text 
SLO = 99.9% 
Error Budget = 0.1% 
``` 

For 3,000,000 valid requests: 

```text 
Allowed Failed Requests = 3,000,000 × 0.001 
 = 3,000 
``` 

### 5. Rationale Behind the Target 

Explain why the selected target is appropriate. 

The rationale may consider: 

- User expectations 
- Business impact 
- Technical limitations 
- Operational effort 
- Cost 
- Feature velocity 
- Historical service behaviour 

### 6. Review Schedule 

Define how regularly the SLO will be reviewed. 

During a review, teams should check: 

- Whether the SLI still represents the user experience 
- Whether the target remains realistic 
- Whether the measurement source is accurate 
- Whether the error budget policy is effective 
- Whether the SLO influences engineering decisions 

--- 

## Making SLOs Actionable 

An SLO provides value only when stakeholders agree to enforce its corresponding error budget policy. 

An effective SLO should influence: 

- Engineering priorities 
- Release decisions 
- Reliability improvements 
- Incident follow-up 
- Operational planning 
- Risk management 

If SLO violations do not lead to decisions or corrective actions, the SLO becomes only a reporting number. 

> A useful SLO connects user experience, technical measurements, and engineering decisions. 

--- 
--- 

## Dashboards and Reports 

Alongside SLOs and Error Budget Policies, dashboards and reports play a critical role in helping teams understand service reliability. 

Benefits include: 

- Making SLI performance visible 
- Tracking error budget consumption 
- Identifying reliability trends 
- Supporting decision-making during incidents 
- Communicating service health to stakeholders 

A good dashboard should make SLO status clear and easy to understand. 

--- 

## Continuous Improvement of SLOs 

SLOs should not be treated as static targets. 

Every service can benefit from continuous SLO improvement as: 

- Systems evolve 
- User expectations change 
- Business requirements shift 
- Monitoring capabilities improve 

Reliability engineering is an iterative process rather than a one-time activity. 

### Improving SLO Quality 

A good SLO should correlate with real customer impact and service incidents. 

To validate an SLO: 

- Compare outages against SLO violations 
- Review support tickets and customer complaints 
- Analyze error budget consumption 
- Investigate incidents that were not detected by the SLO 

Real incidents provide valuable feedback for refining reliability measurements. 

### Improving SLO Coverage 

If important incidents occur without affecting the SLO, then the SLO is not measuring enough of the user experience. 

Possible improvements include: 

- Expanding measurement coverage 
- Adding additional SLIs 
- Improving monitoring sources 
- Measuring closer to actual user interactions 

Coverage should represent as much of the real user experience as possible. 

### Relaxing and Tightening SLOs 

Not every SLO target remains appropriate forever. 

#### Tighten an SLO when: 

- Important incidents are not being detected 
- Reliability expectations increase 
- User experience requires stricter guarantees 

#### Relax an SLO when: 

- The target creates excessive operational burden 
- Error budgets are consistently too restrictive 
- Alerts are generated for insignificant issues 

The objective is to create a target that is both useful and achievable. 

--- 

## Reducing False Positives and False Negatives 

An effective SLO should detect genuine reliability problems while avoiding unnecessary alerts. 

### False Positive 

A false positive occurs when: 

```text 
The system reports a reliability problem that users are not actually experiencing. 
``` 

Result: 

- Unnecessary investigations 
- Alert fatigue 
- Reduced trust in monitoring 

### False Negative 

A false negative occurs when: 

```text 
Users experience a problem but the monitoring system fails to detect it. 
``` 

Result: 

- Undetected user impact 
- Delayed response 
- Increased risk 

### Desired Goal 

Aim for: 

- High precision 
- High recall 

This helps the SLO accurately represent the real user experience. 

--- 

## Aspirational SLOs 

An aspirational SLO represents a future reliability goal rather than a target that can currently be achieved. 

Benefits include: 

- Creating a long-term reliability vision 
- Driving engineering improvements 
- Guiding future investment decisions 

Aspirational SLOs should not be confused with current operational targets. 

--- 

## Iterate Continuously 

Do not wait for a perfect SLO before implementation. 

A better approach is: 

1. Start with a reasonable SLO. 
2. Gather feedback from production. 
3. Improve measurements. 
4. Refine targets. 
5. Repeat. 

Reliability engineering is fundamentally iterative. 

--- 

## Decision-Making Using SLOs and Error Budgets 

Once SLOs and error budgets are established, they should influence engineering decisions. 

### Error Budget Exhausted 

When the error budget is consumed: 

- Slow down feature releases 
- Increase focus on reliability work 
- Investigate the root causes of reliability issues 
- Strengthen testing and monitoring 

The purpose is to restore system reliability before introducing additional risk. 

### Severe Reliability Risk 

When reliability concerns become critical, teams may: 

- Declare an operational emergency 
- Pause risky deployments 
- Re-architect unstable components 
- Improve testing strategies 
- Enhance monitoring coverage 

### Prioritizing Incidents 

Incidents should be classified according to: 

- Business impact 
- User impact 
- Reliability risk 

This helps allocate engineering effort effectively. 

--- 

## Advanced Topics 

### Modeling User Journeys 

Users care about successfully completing actions rather than individual system components. 

Examples include: 

- Signing in 
- Performing a search 
- Completing a payment 
- Uploading a file 

SLOs should increasingly focus on measuring outcomes that matter to users. 

--- 

### Grading Interaction Importance 

Not every request has the same level of importance. 

Examples: 

- Login failures may be more important than recommendation failures. 
- Payment failures may be more important than profile updates. 

Different user interactions may require different SLO targets. 

One common approach is request bucketing, where interactions are grouped according to importance. 

--- 

### Modeling Dependencies 

Most systems rely on downstream services. 

Examples: 

- Databases 
- External APIs 
- Authentication providers 
- Messaging systems 

Reliability measurements should account for dependencies because downstream failures can affect user experience. 

Common strategies include: 

- Caching 
- Retries 
- Graceful degradation 
- Redundancy 

Teams should also watch for: 

- Shared failure domains 
- Common dependencies 
- Single points of failure 

--- 

### Dependency-Caused Outages 

Organizations should decide in advance how dependency failures affect error budgets. 

Questions include: 

- Does a dependency outage consume our error budget? 
- Is the dependency considered part of the service? 
- How should responsibility be allocated? 

These policies should be agreed upon before incidents occur. 

--- 

### Experimenting with SLOs 

Reliability targets should occasionally be evaluated and challenged. 

Experiments can help answer questions such as: 

- Does a stricter SLO create meaningful business value? 
- Is the current target too expensive to maintain? 
- Would different measurement approaches improve accuracy? 

Experiments should only be conducted when sufficient error budget exists. 

--- 
## Key Takeaways

- An **SLI** measures service performance.
- An **SLO** defines the desired reliability target.
- An **error budget** represents the acceptable level of failure.
- A 100% reliability target is usually impractical and can limit development.
- SLIs should measure outcomes that matter to users.
- Error budgets should influence engineering and release decisions.
- SLOs and their implementations should be reviewed and refined using production feedback.
- Divide the system into request-driven, pipeline, and storage components before selecting SLIs. 
- An SLI specification defines **what** should be measured. 
- An SLI implementation defines **how** the measurement is collected. 
- Measurement sources may include application logs, load balancers, black-box probes, and client-side instrumentation. 
- Rolling windows are closely connected to current user experience. 
- Calendar windows are useful for fixed business reporting and planning. 
- Short windows support quick operational decisions. 
- Long windows support strategic reliability analysis. 
- Product, Development, and SRE stakeholders must agree on the SLO. 
- An error budget policy defines the response to excessive unreliability. 
- Common responses include prioritizing reliability bugs, pausing feature development, and applying a production freeze. 
- SLO documentation should include the service description, target, SLI implementation, error budget calculation, rationale, and review schedule. 
- An SLO is valuable only when it influences real engineering decisions.

> Reliability is not about eliminating every failure. It is about defining acceptable reliability and making informed decisions within that boundary.
> 
---
# Chapter Summary 

SLOs provide a structured way to measure and manage service reliability. 

Key concepts covered: 

- Service Level Indicators (SLIs) 
- Service Level Objectives (SLOs) 
- Error Budgets 
- SLI Specification vs Implementation 
- SLO Time Windows 
- Stakeholder Agreements 
- Error Budget Policies 
- Dashboards and Reporting 
- Continuous SLO Improvement 
- User Journey Modeling 
- Dependency Management 

The central idea of SLO Engineering is not to eliminate all failures, but to define acceptable reliability levels and use data-driven decisions to balance reliability, feature development, and operational effort. 

> SLOs measure reliability, while Error Budgets help teams make informed trade-offs between reliability and innovation. 
``
