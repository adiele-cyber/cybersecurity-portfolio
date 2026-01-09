# Log Analysis: Authentication Events

## Objective
Analyze authentication logs to identify suspicious access activity.

## Data Sources
- Linux authentication logs
- Wazuh-generated alerts

## Analysis
Multiple failed login attempts were observed from a single source
over a short time interval, deviating from normal authentication behavior.

## Assessment
The pattern indicated an attempted brute-force authentication attack.

## Conclusion
The activity was classified as malicious and would warrant escalation
and mitigation actions in a production environment.
