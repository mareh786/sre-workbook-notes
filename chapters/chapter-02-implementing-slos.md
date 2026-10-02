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

## Key Takeaways

- An **SLI** measures service performance.
- An **SLO** defines the desired reliability target.
- An **error budget** represents the acceptable level of failure.
- A 100% reliability target is usually impractical and can limit development.
- SLIs should measure outcomes that matter to users.
- Error budgets should influence engineering and release decisions.
- SLOs and their implementations should be reviewed and refined using production feedback.

> Reliability is not about eliminating every failure. It is about defining acceptable reliability and making informed decisions within that boundary.
