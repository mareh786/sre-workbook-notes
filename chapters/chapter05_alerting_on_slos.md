# Chapter 5: Alerting on SLOs 

## Introduction 

The main focus of SLO-based alerting is to turn Service Level Objectives into workable alerts. 

When an issue occurs, an appropriate alert can help significantly in restoring the service. 

--- 

# Alerting Conditions 

The purpose of an alert is to notify teams about significant events. 

There are four important factors to consider while creating alerts from SLOs: 

1. Precision 
2. Recall 
3. Detection time 
4. Reset time 

An ideal alert maintains the right balance among all four factors. 

--- 

## 1. Precision 

Precision is the proportion of detected events that are actually significant. 

A high-precision alert means that when a notification is received, the event is likely to have a real impact on the error budget. 

```text 
Precision = Significant alerts / Total alerts 
``` 

High precision reduces unnecessary or irrelevant alerts. 

--- 

## 2. Recall 

Recall is the proportion of significant issues that successfully generate alerts. 

If recall is 100%, every significant issue is detected and alerted. 

```text 
Recall = Significant issues alerted / Total significant issues 
``` 

High recall reduces the chance of missing issues that may affect the error budget. 

--- 

## 3. Detection Time 

Detection time is the amount of time required for an alert to trigger after a problem begins. 

Long detection times may cause important issues to be missed or detected too late, which can negatively affect the error budget. 

--- 

## 4. Reset Time 

Reset time is the amount of time required for an alert to return to its normal state after the issue is resolved. 

A long reset time may create confusion and can eventually cause new issues to be ignored. 

--- 

# Ways to Alert on Significant Events 

There are six approaches to setting up alerts based on error-budget consumption. 

--- 

## Approach 1: Alert on Error Rate 

In this approach, the current error rate is calculated. An alert is triggered when the error rate exceeds the error-budget threshold. 

For example, if the monthly error budget is `0.1%`, the rate of errors can be checked over short time windows. 

### Advantages 

- High recall 
- Fast detection 

### Disadvantage 

- Poor precision 

This approach may generate alerts for short-lived error spikes that do not significantly consume the error budget. 

--- 

## Approach 2: Increase the Alert Window 

In this approach, the alerting window is increased from a short period to several hours. 

An alert is triggered only if the problem persists throughout the longer window. 

This reduces noise and provides better precision than alerting on short windows. 

### Advantages 

- Better precision than the first approach 
- Reduced alert noise 

### Disadvantages 

- Slower detection 
- Poor reset time 

For example, an alert configured with a long window may trigger only after several hours and may continue for many hours after the issue is resolved. 

--- 

## Approach 3: Add Duration to Alerting 

In this approach, an alert is triggered only when the condition remains true for a defined period. 

### Advantage 

- Improved precision 

### Disadvantages 

- Slow detection 
- Fluctuating errors may repeatedly reset the timer 

If the error rate fluctuates, the alerting duration may keep restarting before the alert can trigger. 

--- 

## Approach 4: Alert on Burn Rate 

Burn rate represents how quickly the error budget is being consumed. 

For example: 

- A burn rate of `1` means the error budget is being consumed at the expected rate. 
- A burn rate of `10` means the budget is being consumed ten times faster and may be exhausted much sooner. 

A useful relationship is: 

```text 
Error-rate threshold = (1 - SLO) × Burn rate 
``` 

The approximate fraction of error budget consumed during an alerting window can be represented as: 

```text 
Error budget consumed = Burn rate × Alerting window / SLO period 
``` 

By defining the alerting-window size and deciding how much error-budget consumption is significant enough to notify the team, the burn rate can be calculated and used for alerting. 

### Advantages 

- Good precision 
- Faster detection 
- Better reset time 
- Alerting is directly connected to error-budget consumption 

--- 

## Approach 5: Multiple Burn-Rate Alerts 

Alerting can be improved by using multiple burn rates with different time windows. 

If the specified threshold is crossed, the relevant alert is triggered. 

This allows different responses for different severity levels: 

- Urgent issues can trigger paging alerts. 
- Moderate issues can create lower-priority alerts. 
- Slow error-budget consumption can be tracked through tickets. 

### Advantages 

- Good precision 
- Good recall 
- Supports different severity levels 

--- 

## Approach 6: Multiwindow, Multi-Burn-Rate Alerts 

This approach extends multiple burn-rate alerting by adding a shorter window to confirm whether the error budget is still being consumed before triggering the alert. 

The longer window identifies meaningful error-budget consumption, while the shorter window confirms that the issue is still active. 

### Advantages 

- High precision 
- High recall 
- Fast detection 
- Better reset behaviour 

This is the most balanced approach among the alerting methods covered in these notes. 

--- 

# Comparison of Alerting Approaches 

| Approach | Precision | Recall | Detection | Reset | 
|---|---|---|---|---| 
| Error-rate alert | Low | High | Fast | Fast | 
| Increased alert window | Better | High | Slow | Slow | 
| Alert with duration | Better | May miss fluctuating issues | Slow | Depends on duration | 
| Burn-rate alert | Good | Good | Fast | Better | 
| Multiple burn-rate alerts | Good | Good | Based on severity | Good | 
| Multiwindow, multi-burn-rate alerts | High | High | Fast | Fast | 

--- 

# Low-Traffic Services and Error-Budget Alerting 

The multiwindow, multi-burn-rate approach works better for high-traffic services. However, it may be less effective for services with low traffic. 

For example, if a service receives only ten requests and one request fails, the monitoring system may immediately trigger an alert. 

This creates difficulties because a small number of failures can produce a high error rate, even when the total traffic is very low. 

--- 

## Remediation for Low-Traffic Services 

Several approaches can be used to compensate for the lack of real traffic. 

### 1. Generate Artificial Traffic 

Artificial or synthetic traffic can be generated to compensate for low real-user traffic. 

This provides more requests for evaluating service behaviour and alerting conditions. 

### 2. Combine Smaller Services 

A few low-traffic and related services can be combined into a single monitoring unit. 

This increases the total traffic volume and may allow significant events to be detected more precisely. 

### 3. Reduce the Impact of Individual Failures 

Alerting rules can be modified so that a single failed request has less impact on the alerting decision. 

--- 

## Difficulties with Artificial Traffic 

Artificial traffic also introduces limitations. 

- Synthetic traffic may not accurately represent real user behaviour. 
- Real user issues may remain hidden when synthetic traffic continues to succeed. 

Therefore, synthetic traffic should not be treated as a complete replacement for real-user monitoring. 

--- 

# Combining Different Services 

A few related low-traffic services can be combined and monitored as one unit. 

Combining services can help detect significant events more precisely by increasing the amount of traffic available for evaluation. 

However, combining multiple services may make it harder to identify issues when one component fails completely. 

--- 

# Extreme Availability Goals 

The previously discussed alerting methods may fail when a service has either: 

- A relatively low availability SLO, such as `90%` 
- An extremely high availability SLO, such as `99.999%` 

In both cases, the monitoring and alerting system may fail to generate a useful alert. 

--- 

## Low Availability SLO 

Consider a service with a `90%` availability SLO. 

If an alert is scheduled when `2%` of the error budget is consumed, a complete one-hour outage may consume only approximately `1.4%` of the error budget. 

As a result, the alert may never trigger. 

--- 

## Extremely High Availability SLO 

Consider a service with a `99.999%` availability SLO. 

During a complete outage, the entire error budget may be consumed in approximately `26 seconds`. 

The processes of: 

- Collecting metrics 
- Evaluating the alerting rule 
- Triggering the alert 

may take longer than the time required to exhaust the error budget. 

In such a case, the alert may provide little or no value. 

--- 

## Design Consideration 

For extreme availability targets, the system should be designed so that the possibility of a complete outage is extremely low. 

Alerting alone cannot compensate for a system that is capable of rapidly consuming its entire error budget. 

--- 

# Alerting at Scale 

As an application scales, its alerting system must also be designed to scale. 

Different types of services may require different: 

- Alerting windows 
- Burn rates 
- Alert severities 
- Reliability targets 

A single alerting configuration may not be suitable for every service. 

--- 

## Alerting Buckets 

Services and requests can be grouped into different alerting buckets. 

### Critical 

Used for the most important issues. 

These alerts require immediate attention. 

### High-Fast 

Used for requests requiring: 

- High availability 
- Low latency 
- Rapid response 

### High-Slow 

Used for important requests that are less sensitive to latency. 

### Low 

Used for less important requests where an outage may still be visible to users. 

### No SLO 

Used for features that are nearly invisible to users or do not require a formal SLO. 

These buckets provide sufficient fidelity for protecting user experience without creating an alerting system that is too complicated or expensive to manage. 

--- 

# Making Service and Infrastructure Changes 

Systems can also be changed to make alerting less sensitive to individual failures. 

Instead of alerting on one failure, the system can be designed to require multiple failures before triggering an alert. 

For example: 

```text 
Alert after three relevant failures instead of one failure. 
``` 

This reduces the impact of a single failed request. 

--- 

# Lowering SLOs or Increasing the Alert Window 

Another possible approach is to adjust the reliability target or alerting window. 

## Lower the SLO 

For example, an SLO may be reduced from: 

```text 
99.9% → 99% 
``` 

This provides an additional `0.9%` error budget to absorb minor failures. 

The SLO should remain realistic and aligned with the service’s actual importance. 

## Increase the Alerting Window 

The alerting window may be increased so that alerts are generated for issues that persist over a longer period. 

This can help distinguish sustained problems from short-lived failures. 

--- 

# Overall Strategies 

A combination of the following approaches may be used: 

- Generate artificial traffic. 
- Reduce the impact of individual failures. 
- Combine small and related services. 
- Choose realistic SLOs. 
- Increase alerting windows when appropriate. 
- Use different alert windows and burn rates for different service types. 
- Group services or requests into alerting buckets. 

--- 

# Conclusion 

Meaningful SLOs combined with appropriate alerting help teams respond quickly when a service is threatened. 

Multiwindow, multi-burn-rate alerting is generally the most effective and balanced alerting technique because it provides: 

- High precision 
- High recall 
- Fast detection 
- Fast reset 

However, the alerting strategy must be adapted according to: 

- Service traffic 
- Service importance 
- Availability targets 
- Latency sensitivity 
- Error-budget size 

No single alerting configuration is suitable for every service. 

--- 

# Key Takeaways 

- SLO-based alerting should focus on meaningful threats to the error budget. 
- A useful alert balances precision, recall, detection time, and reset time. 
- Error-rate alerts provide fast detection and high recall but may have poor precision. 
- Longer alert windows improve precision but increase detection and reset times. 
- Duration-based alerts reduce noise but can miss fluctuating issues. 
- Burn rate describes how quickly the error budget is being consumed. 
- Multiple burn-rate alerts support different issue severities. 
- Multiwindow, multi-burn-rate alerting combines longer and shorter windows. 
- Multiwindow, multi-burn-rate alerting provides the best overall balance among the approaches discussed. 
- Burn-rate alerting works better when a service receives sufficient traffic. 
- Low-traffic services may alert because of a very small number of failed requests. 
- Artificial traffic can compensate for low traffic but may not represent real user behaviour accurately. 
- Synthetic monitoring can continue succeeding while real users experience problems. 
- Related low-traffic services can be combined into a single monitoring unit. 
- Alert rules can be modified to reduce the impact of one failed request. 
- Extreme SLO targets create special alerting problems. 
- A low availability target may allow a major outage without consuming enough error budget to trigger an alert. 
- An extremely high availability target may exhaust the error budget before monitoring can generate an alert. 
- Highly available services must be designed to minimize the possibility of complete outages. 
- Alerting systems must scale along with applications and services. 
- Different services may require different alert windows and burn rates. 
- Alerting buckets can classify services as Critical, High-Fast, High-Slow, Low, or No SLO. 
- Adjusting the SLO or increasing the alerting window may improve alert quality. 
- Realistic SLOs should reflect user impact, service importance, and operational requirements. 
- The best alerting strategy may combine artificial traffic, service grouping, realistic SLOs, and appropriate alert windows.
