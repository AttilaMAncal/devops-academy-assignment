# Section 3 - Task A: Production went down

## Scenario

The application is reported as down in production.

## My investigation process

### 1. Confirm the incident
First, I would verify whether the issue is real and determine its scope.

I would check:
- monitoring dashboards
- alerting system
- uptime checks
- user-facing symptoms
- whether the issue affects all users or only part of the system

Goal:
- confirm the outage
- understand impact
- determine severity

### 2. Assess business impact
I would quickly determine:
- which service is affected
- whether the outage is complete or partial
- how many users are impacted
- whether revenue-critical or customer-facing functions are down

This helps define incident priority and escalation level.

### 3. Start incident communication
If the outage is real, I would communicate early.

I would:
- notify the team
- open or join the incident channel
- assign an incident owner if needed
- provide a short status update with known facts only

Example:
- what is broken
- when it started
- current impact
- who is investigating

### 4. Check recent changes first
My first technical suspicion would be recent change activity.

I would check:
- recent deployments
- infrastructure changes
- configuration changes
- secrets or environment variable changes
- DNS / certificate / networking changes

Reason:
Many production incidents are caused by recent changes, so this is usually the fastest way to find a likely root cause.

### 5. Check service health
I would examine the affected application and its dependencies.

I would check:
- application health endpoints
- container or pod status
- restart counts
- CPU and memory usage
- node health
- database connectivity
- cache / message queue health
- external dependency status

### 6. Inspect logs and errors
Next, I would review logs to identify the failure pattern.

I would check:
- application logs
- web server / reverse proxy logs
- Kubernetes events if applicable
- system logs
- CI/CD deployment logs

I would look for:
- crash loops
- timeouts
- connection refusals
- authentication failures
- out-of-memory errors
- dependency failures

### 7. Decide whether rollback is safer than debugging live
If the issue started immediately after a deployment or config change, I would strongly consider rollback.

I would choose rollback when:
- the likely cause is a recent release
- rollback is low risk
- recovery speed is more important than immediate root cause analysis

My priority would be service restoration first, then deeper investigation.

### 8. Apply the fastest safe mitigation
Depending on findings, I would take the safest action to restore service.

Examples:
- rollback deployment
- restart failed pods or services
- scale up instances
- fix broken config or secret reference
- fail over to a healthy region or replica
- temporarily disable a problematic feature flag

I would avoid risky changes without evidence.

### 9. Verify recovery
After mitigation, I would verify that the service is actually healthy.

I would confirm:
- alerts are cleared
- error rates are dropping
- latency is normal
- key user flows work
- health checks pass
- no secondary systems are failing

### 10. Continue communication
I would keep stakeholders updated during the incident.

Updates should include:
- current status
- actions taken
- whether service is restored
- known remaining risks
- next update time if needed

### 11. Perform root cause analysis after stabilization
Once production is stable, I would document:
- timeline
- root cause
- contributing factors
- why detection did or did not happen quickly
- what prevented faster recovery

### 12. Define prevention actions
Finally, I would create follow-up actions such as:
- better monitoring and alerting
- stronger health checks
- safer deployment strategy
- rollback automation
- improved runbooks
- better testing for configuration changes
- capacity and dependency monitoring

## Key principles

1. Restore service before over-optimizing the investigation.
2. Communicate early and clearly.
3. Check recent changes first.
4. Use evidence from monitoring, logs, and health checks.
5. Avoid making random changes in production.
6. Document what happened and improve the system afterward.

## Short answer

If production goes down, I would first confirm the outage and assess the business impact. Then I would communicate with the team, check recent changes, inspect service health and logs, and apply the fastest safe mitigation such as a rollback. After recovery, I would verify system health, communicate status, and run a proper root cause analysis with prevention actions.