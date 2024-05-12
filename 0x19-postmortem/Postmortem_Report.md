# Postmortem: E-commerce Checkout Outage

## Issue Summary

### Duration:  30 minutes (10:00 AM PST - 10:30 AM PST)

Impact: The e-commerce checkout process was unavailable for 30 minutes. Users attempting to checkout encountered an error message and were unable to complete their purchases. This impacted approximately 10% of users during the peak morning traffic hour.

Root Cause: A memory leak in the shopping cart service caused the server to crash.

### Timeline

    10:00 AM PST: Monitoring alert triggered due to a surge in application errors on the shopping cart service.
    10:01 AM PST: On-call engineer investigates the alert and identifies the shopping cart service as the source of errors. Initial suspicion falls on a recent code deployment.
    10:05 AM PST: Engineer reviews deployment logs and code changes. No anomalies are found.
    10:10 AM PST: Engineer examines server logs and discovers high memory usage on the shopping cart service.
    10:15 AM PST: The incident is escalated to the development team lead. A decision is made to restart the shopping cart service to recover from the potential memory leak.
    10:20 AM PST: The shopping cart service is restarted. The service recovers and checkout functionality is restored.
    10:25 AM PST: The development team begins a deeper investigation into the cause of the memory leak.
    10:30 AM PST: Checkout functionality confirmed to be fully operational.

### Root Cause and Resolution

Further investigation by the development team revealed a bug in a recently added shopping cart functionality. This bug caused unnecessary data to be stored in memory for each active shopping cart, leading to a gradual memory leak. The leak eventually reached a critical point, causing the server to crash.

The development team identified the specific code responsible for the leak and implemented a fix to properly manage memory allocation. The fix was thoroughly tested and deployed to the production environment.

### Corrective and Preventative Measures

    Improve code review process: The code review process will be enhanced to include a specific focus on memory management practices.
    Implement unit testing for memory usage: Unit tests will be developed to specifically assess the memory footprint of new code features.
    Enhanced monitoring for memory usage: Monitoring for application memory usage will be configured to trigger alerts at lower thresholds to provide earlier warnings of potential issues.
    Regular server health checks: A scheduled task will be implemented to automatically restart the shopping cart service on a regular basis to mitigate the impact of potential memory leaks.

By implementing these corrective and preventative measures, we aim to significantly reduce the risk of future outages caused by memory leaks and improve the overall stability of the e-commerce platform.
