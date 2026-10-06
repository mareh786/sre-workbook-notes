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

--- 

## Key Point 

Metrics detect problems and logs explain problems. 

Therefore, a mature monitoring strategy uses both together.
