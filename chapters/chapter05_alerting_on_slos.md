
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
