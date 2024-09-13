Issue Summary:

Duration:
July 15, 2024,9:00 AM - 11:45 AM UTC (2 hours 45 minutes)
Impact:
Our e-commerce website was partially down, with 65% of users experiencing slow load times, intermittent outages, or being completely unable to process orders. Affected users faced 504 Gateway Timeout errors during checkout, leading to a significant drop in conversion rates

Root Cause:
An unexpected surge in traffic during a flash sale overwhelmed our database connection pool, causing cascading failures across our web servers.

Timeline:
	9:05 AM: Monitoring alert triggered, indicating a spike in 504 errors.
	9:10 AM: Engineer A manually reviewed server logs, noticing multiple timeouts originating from the checkout service.
	9:20 AM: Assumed root cause was a misconfiguration in Nginx due to recent updates. Attempts were made to revert changes.
	9:30 AM: No improvement. The issue was escalated to the DevOps team for further investigation.
	9:45 AM: Database metrics were examined, revealing abnormally high connection usage.
	10:00 AM: Misleading assumption that a third-party payment gateway integration was causing the overload, leading to temporary service suspension.
	10:30 AM: Issue persisted. The Database Administrator (DBA) team was engaged.
	10:45 AM: DBA identified maxed-out database connection pools.
	11:00 AM: Connection pool limits were increased, and the checkout service was optimized. 	11:45 AM: Full service restoration. Performance stabilized and error rates dropped to normal levels


Root Cause and Resolution:
Root Cause:
The issue stemmed from an overwhelming influx of user traffic during a high-demand flash sale, which exhausted the available database connection pool. The checkout service, which relies heavily on real-time database queries, couldn't acquire connections in time, causing it to fail and trigger 504 errors on the front end. This, in turn, affected both user experience and transaction processing.
Resolution:
The immediate solution was to increase the maximum connection pool size for our database 
and optimize the checkout service by reducing the number of database queries per transaction. Additionally, a temporary rate-limiting measure was applied to control the incoming traffic until the system could scale appropriately. Once these changes were implemented, service was restored
Corrective and Preventive Measures:
Improvements:
	Better handling of sudden traffic spikes by auto-scaling database resources.
	Improved monitoring on critical services, like checkout and database, to catch issues earlier.
TODO List:
1.	Patch Nginx Configuration: Ensure that the Nginx server's timeout settings can handle higher traffic spikes without failure.
2.	Add Database Connection Monitoring: Implement real-time alerts for database connection pool usage to prevent exhaustion.
3.	Optimize Database Queries: Audit and optimize all database queries used in high-traffic services like checkout.
4.	Auto-Scaling for Database: Enable automated scaling for database resources during periods of increased load.
5.	Stress Testing: Conduct stress tests to simulate traffic spikes and measure the system's response to future flash sales.
In conclusion, this outage taught us the importance of proactive scaling, proper resource monitoring, and thorough testing during high-traffic events.


