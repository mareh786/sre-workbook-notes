# SRE Workbook Notes

Personal notes, summaries, key concepts, and practical takeaways from **_The Site Reliability Workbook_** by Google.

This repository documents my learning journey through Site Reliability Engineering (SRE), with a focus on understanding the concepts and connecting them to real-world DevOps and Kubernetes operations.

> **Note:** These are personal study notes and summaries. They are not a replacement for the original book.

---

## 📖 About the Book

**The Site Reliability Workbook** is a practical guide to applying Site Reliability Engineering principles in real-world organizations.

It covers topics such as:

- Service Level Objectives (SLOs)
- Error Budgets
- Monitoring and alerting
- Incident management
- Toil reduction
- Automation
- Reliability culture
- SRE implementation
- Organizational practices

The book builds upon the ideas introduced in **_Site Reliability Engineering: How Google Runs Production Systems_** and focuses more heavily on practical implementation.

---

## 🎯 Purpose of This Repository

The goal of this repository is to:

- Summarize important concepts from each chapter
- Build a structured SRE knowledge base
- Reinforce learning through writing
- Connect SRE concepts with DevOps and Kubernetes
- Record practical examples and lessons learned
- Create a reference that can be revisited during future projects and interviews

The notes are written in my own words wherever possible, with emphasis on **understanding rather than memorization**.

---

## 📚 Chapter Notes

| Chapter | Topic | Status |
|---|---|---|
| Chapter 1 | How SRE Relates to DevOps | ✅ Completed |
| Chapter 2 | Introduction to SLOs | 🔄 In Progress |
| Chapter 3 | | ⏳ |
| Chapter 4 | | ⏳ |
| Chapter 5 | | ⏳ |
| Chapter 6 | | ⏳ |
| ... | ... | ⏳ |

> Chapter titles and content will be added as I progress through the book.

---

## 🧠 Chapter 1 — How SRE Relates to DevOps

The first chapter explores the relationship between **DevOps** and **Site Reliability Engineering**.

### Core idea

> **SRE implements DevOps.**

DevOps can be understood as a philosophy and set of practices focused on collaboration, automation, continuous improvement, and breaking down organizational silos.

SRE applies these principles through **software engineering techniques to solve operational and reliability problems**.

### Key concepts covered

- DevOps and its background
- The CALMS framework
- Eliminating organizational silos
- Blameless culture
- Small incremental changes
- Measurement and meaningful metrics
- SRE as an implementation of DevOps principles
- SLOs and error budgets
- Toil reduction
- Automation
- Learning from failures
- Shared ownership
- Common tooling
- DevOps vs SRE
- Organizational considerations for adopting SRE

### Important SRE concepts

```text
DevOps
  │
  ├── Collaboration
  ├── Automation
  ├── Continuous Improvement
  └── Shared Ownership
          │
          ▼
        SRE
          │
          ├── SLOs
          ├── Error Budgets
          ├── Toil Reduction
          ├── Automation
          └── Software Engineering
```

### Key takeaway

DevOps and SRE are closely related, but they are not identical.

**DevOps provides the principles, while SRE provides a practical engineering approach for applying many of those principles to reliability and operations.**

---

## 🔧 My Learning Approach

For each chapter, I aim to capture:

### 1. Concepts

What does the chapter introduce?

### 2. Why It Matters

Why is the concept important in real production environments?

### 3. Practical Examples

How could the concept be applied to real systems?

### 4. Kubernetes / Cloud Connection

Where applicable, I connect the concepts to:

- Kubernetes
- AWS / GCP
- CI/CD
- Observability
- Monitoring
- Incident response
- Automation
- Infrastructure

### 5. Key Takeaways

A short summary of what I should remember from the chapter.

---

## ☸️ SRE + Kubernetes

As I work primarily with Kubernetes and cloud-native environments, I will also try to connect SRE concepts with practical Kubernetes scenarios.

Examples:

| SRE Concept | Kubernetes Example |
|---|---|
| Reliability | Highly available workloads |
| SLO | API availability target |
| Error Budget | Allowed service downtime |
| Toil | Repetitive manual cluster operations |
| Automation | Automated remediation |
| Monitoring | Prometheus / Grafana |
| Alerting | SLO-based alerts |
| Incident Response | Pod/node/service failures |
| Self-Healing | Kubernetes controllers |
| Capacity Planning | Resource requests and limits |

---

## 📌 Important Concepts to Track

As I progress through the book, this repository will build a reference around:

- SRE
- DevOps
- SLOs
- SLIs
- SLAs
- Error Budgets
- Toil
- Monitoring
- Alerting
- Incident Management
- Postmortems
- Automation
- Capacity Planning
- Reliability
- Scalability
- Availability
- Performance
- Observability
- Service Ownership

---

## 📈 Learning Progress

```text
The Site Reliability Workbook
            │
            ▼
      Chapter 1
   DevOps → SRE
            │
            ▼
      Chapter 2
        SLOs
            │
            ▼
       Chapters ...
            │
            ▼
    Practical SRE Concepts
            │
            ▼
 Kubernetes / Cloud / DevOps
            │
            ▼
      Real-world Practice
```

---

## 🛠️ Future Additions

As I progress, I plan to add:

- Chapter summaries
- SRE terminology
- Practical examples
- Kubernetes use cases
- Monitoring and observability examples
- SLO examples
- Incident-response scenarios
- Automation ideas
- Personal learnings
- Useful references
- Interview-oriented revision notes

---

## 📂 Repository Structure

```text
sre-workbook-notes/
│
├── README.md
│
├── chapters/
│   ├── chapter-01-devops-and-sre.md
│   ├── chapter-02-slos.md
│   └── ...

```

---

## 📖 Primary Reference

**The Site Reliability Workbook: Practical Ways to Implement SRE**

Google / O'Reilly

These notes are intended to complement the book, not replace it.

---

## 👨‍💻 Why I'm Learning SRE

My goal is to develop a stronger understanding of how reliable production systems are designed, operated, monitored, and continuously improved.

Coming from a **DevOps and Kubernetes** background, SRE provides a useful framework for connecting:

```text
Infrastructure
      +
Automation
      +
Observability
      +
Software Engineering
      +
Reliability
      =
Better Production Systems
```

---

## ⭐ Final Goal

The objective isn't simply to finish the book.

The objective is to be able to take the principles discussed in the book and apply them to **real production systems**.

> **Read → Understand → Document → Apply → Reflect**
