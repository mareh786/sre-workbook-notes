
# Chapter 4: Monitoring 

Metrics and logs are two fundamental sources of monitoring for any set of services. 

Monitoring allows us to have visibility into a system to: 

- Check the health of services 
- Diagnose when things go wrong 

--- 

# Desirable Features of a Monitoring Strategy 

## 1. Speed 

This is a crucial feature because it defines freshness and the speed of retrieval. 

Stale data (if it is many delayed also) can cause a significant impact between cause and effect. 

It can lead to assumptions of false correlations. 

--- 

## 2. Calculations 

Monitoring systems must do useful calculations rather than only data storage. 

Some data may look unimportant today, but over time it may become useful. 

It must support: 

- Long-term storage 
- Counters for monotonically increasing values 
- Rates 
- Averages 
- Percentiles for viewing long tails 
- Storage of raw data for trend analysis (offline) 

--- 

## 3. Interface 

Dashboards are the primary interface. 

Usually SREs and developers make decisions from dashboards. 

Sometimes we need to create different dashboards from the same data to increase understandability across teams. 

However: 

- Dashboard layout 
- Naming conventions 
- Graph styles 

should remain similar to reduce confusion across teams. 

The interface should also support aggregation by: 

- Server versions 
- Machine types 
- Request types 
- Etc. 

It should also allow ad-hoc drill-downs for troubleshooting. 

--- 

## 4. Alerts 

Alerts must be triggered according to priority. 

High-priority alerts must be addressed as an emergency. 

To reduce duplicate alerts or downstream alerts, an alert-suppression mechanism is required. 

--- 

There are many open-source monitoring systems available. 

We can choose any of them, but self-managed tools provide more control. 

--- 

# Sources of Monitoring Data 

## Metrics 

Metrics are numerical data collected over time. 

They are usually: 

- Lightweight 
- Close to real-time 

and are best suited for dashboards. 

--- 

## Logs 

Logs are appended real-time event records. 

Logs are structured and have timestamps attached to them. 

--- 

# Metrics vs Logs 

| Metrics | Logs | 
|----------|----------| 
| Near real-time | Detailed data | 
| Fast dashboards | Root cause analysis | 
| Alerting | Historical investigation | 
| Trend analysis | Accurate reporting |

Metrics detect problems and logs explain problems. 

Therefore, a mature monitoring strategy uses both together. 

--- 

# Managing Your Monitoring System 

## Treat Your Configuration as Code 

Monitoring component configurations should be treated as code. 

Version control should be enabled for monitoring configurations. 

### Benefits 

- Version history 
- Easier rollback 
- Better traceability 

Apply the same engineering practices used for application code to monitoring configurations. 

--- 

## Encourage Consistency 

Use common: 

- Metrics 
- Naming conventions 
- Dashboards 

Benefits: 

- Improved understanding 
- Easier debugging across teams 

--- 

## Prefer Loose Coupling 

Monitoring components such as: 

- Metric collectors 
- Storage systems 
- Alerting systems 

should be loosely coupled. 

Benefits: 

- Reduced upgrade risk 
- Easier migrations 
- Easier component replacement 

--- 

# Metrics with Purpose 

Every metric should exist for a specific purpose. 

To support SLO-based alerting, the relevant SLI metrics must be available and visible on dashboards. 

Metrics should help with: 

- Alerting 
- Troubleshooting 
- Debugging 
- Root-cause analysis 

--- 

# Monitoring Intended Changes 

Monitor operational changes such as: 

- Service versions 
- Binary versions 
- Important command-line arguments 
- Feature flags 
- Configuration changes pushed to services 

Benefits: 

- Easier identification of issues caused by recent changes 
- Faster root-cause analysis 

If no version change is detected, check timestamps and other configuration changes for relevance. 

--- 

# Monitoring Dependencies 

Always monitor the direct dependencies of an application. 

Track: 

- Request size 
- Response size 
- Latency 
- Response codes 

Recommendations: 

- Instrument common RPC and client libraries for consistent monitoring. 
- Avoid opaque APIs that hide operational monitoring signals. 

--- 

# Monitoring Saturation 

Monitor all critical resources and create alerts before resources reach their limits. 

Key resources include: 

- CPU 
- Memory 
- Thread pools 
- Queues 

Saturation monitoring helps detect resource exhaustion before service failures occur. 

--- 

# Monitoring HTTP Status Codes 

Track all relevant HTTP status codes. 

Monitor: 

- Rate-limited requests 
- Queued requests 
- Denied requests 

HTTP status codes are valuable troubleshooting signals. 

--- 

# Implementing Purposeful Metrics 

Every metric should have a clear purpose. 

### Alerting Metrics 

Used to: 

- Detect problems 
- Trigger alerts 

### Debugging Metrics 

Used to: 

- Investigate incidents 
- Identify root causes 

During postmortems, identify metrics that could have helped diagnose failures faster. 

This helps improve future monitoring coverage. 

--- 

# Testing Alerting Logic 

Alerting rules should be tested before deployment. 

### Validation Steps 

- Verify metrics change under expected conditions. 
- Validate alert-rule evaluation logic. 
- Test alerts before implementation in production environments. 

Testing reduces the risk of: 

- Incorrect alerts 
- Missing alerts 
- Misconfigured alert thresholds 

--- 

# Conclusion 

Monitoring is a core SRE skill. 

Effective monitoring requires: 

- Using both metrics and logs 
- Collecting metrics with a clear purpose 
- Keeping monitoring visible 
- Treating monitoring systems as production applications 
- Continuously improving monitoring practices 

--- 

# Key Takeaways 

- Metrics and logs are the two fundamental sources of monitoring data. 
- Monitoring provides visibility into service health and system behaviour. 
- Monitoring data should be fresh and quickly retrievable. 
- Stale data can lead to false assumptions and incorrect correlations. 
- Monitoring systems should support calculations such as counters, rates, averages, and percentiles. 
- Long-term storage enables trend analysis and historical investigation. 
- Dashboards are the primary interface for observability. 
- Consistent dashboard design reduces confusion across teams. 
- Monitoring systems should support aggregation and ad hoc drill-downs. 
- Alerts should be prioritized according to urgency and impact. 
- Alert suppression helps reduce duplicate and downstream alerts. 
- Metrics are best suited for dashboards, alerting, and trend analysis. 
- Logs provide detailed information for troubleshooting and root-cause analysis. 
- Metrics detect problems; logs explain problems. 
- Monitoring configurations should be managed as code. 
- Consistency in naming, metrics, and dashboards improves collaboration. 
- Monitoring components should be loosely coupled. 
- Every metric should have a clear operational purpose. 
- Service changes and configuration changes should be monitored. 
- Direct application dependencies should always be monitored. 
- CPU, memory, thread pools, and queues are key saturation indicators. 
- HTTP status codes provide important operational signals. 
- Postmortems should drive improvements to monitoring coverage. 
- Alerting logic should always be tested before deployment. 
- Monitoring should be treated as a production system and improved continuously.
