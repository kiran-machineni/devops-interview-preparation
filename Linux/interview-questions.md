# Linux Interview Questions

**1. EC2 instance is running out of disk space. What action will you take to
mitigate the issue?**

- **Understanding the Problem:**
  - Identify what's consuming space: Analyze disk usage with tools like `df -h`
    and `du -sh *`. Large logs, temporary files, or unused data could be
    culprits.
- **Solutions:**
  - **Clean up unnecessary files:** Remove temporary files, old logs, or unused
    packages.
  - **Extend the volume:** If possible, increase the size of the EBS volume
    attached to the instance.
  - **Add a new volume:** Attach an additional EBS volume for more storage.
  - **Optimize application:** Consider refactoring to reduce data storage if the
    application itself is generating excessive files.

**2. What is a bastion host or gateway server? What role do they play?**

- **Definition:** A bastion host is a specially hardened server on a network's
  perimeter. It acts as the sole point of access for external connections to
  systems within a private network.
- **Role:**
  - **Security:** Reduces attack surface by limiting direct access to sensitive
    internal servers.
  - **Monitoring and Auditing:** Centralizes logging of access attempts for
    security analysis.
  - **Two-factor Authentication:** Can implement additional authentication
    layers for enhanced security.

**3. Multiple EC2 instances in an ASG are getting terminated, causing
application downtime. EC2 pricing, quota, all look good. How would you start
debugging this issue?**

- **Troubleshooting Steps:**
  1. **ASG Logs:** Check Auto Scaling Group logs for clues like termination
     reasons or scaling events.
  2. **Instance Termination Logs:** Examine logs on recently terminated
     instances for errors or unexpected shutdowns.
  3. **Application Logs:** Investigate application logs for any indication of
     crashes, errors, or resource exhaustion that could trigger terminations.
  4. **Health Checks:** Verify that ASG health checks are configured correctly
     and reflect actual application health.
  5. **Spot Instance Issues:** If using spot instances, check for price
     fluctuations or spot instance interruptions.

**4. Create a Linux script that will push certain logs to S3 automatically.
Explain the high-level design. Share the steps to achieve this script. Also, the
script should run at a particular time.**

**High-Level Design:**

1. **Log Selection:** Define the specific log files to be pushed to S3.
2. **AWS CLI:** Install the AWS Command Line Interface (CLI) and configure it
   with credentials that have S3 write permissions.
3. **Script:**
   - Use a scripting language like Bash or Python.
   - Implement logic to copy or move the selected log files to a temporary
     location (to prevent issues during upload).
   - Use the `aws s3 cp` or `aws s3 sync` command to transfer the logs to the
     target S3 bucket.
   - Optionally, compress logs before transfer to save storage.
   - Clean up the temporary location.
4. **Scheduling:** Use `cron` (a Linux task scheduler) to trigger your script at
   the desired time (e.g., daily, hourly).

**5. Why Logging is important? What is centralised logging and what tools helps
to achieve centralised logging ?**

- **Troubleshooting:** Logs are your primary source of information when figuring
  out the root cause of errors, failures, or unexpected behavior in systems.
  They provide a detailed timeline of events, states, and error messages.
- **Security Analysis:** Logs reveal patterns of activity that can indicate
  potential security incidents like unauthorized access attempts, intrusion
  attempts, or data breaches.
- **Performance Monitoring:** By analyzing logs, you can track metrics like
  resource usage, response times, and error rates. This helps identify
  performance bottlenecks and optimize your infrastructure.
- **Auditing and Compliance:** Logs provide a historical record of changes to
  your system, which is essential for complying with various regulations that
  require maintaining audit trails.

**What is Centralized Logging?**

Centralized logging means gathering logs from all your sources (servers,
applications, network devices) and storing them in one unified place. This
offers several benefits over traditional distributed logging:

- **Consolidated View:** A central logging platform gives you a complete picture
  of events across your entire infrastructure, making it easier to spot
  patterns.
- **Improved Search and Analysis:** Centralized log systems usually offer
  powerful tools to search, filter, and visualize your logs, allowing you to
  quickly find relevant information.
- **Scalability:** Centralized logging is designed to handle large amounts of
  log data and can easily scale as your infrastructure grows.
- **Enhanced Security:** Centralized storage means you can better protect your
  logs with robust access controls and encryption.

**Tools to Achieve Centralized Logging**

Popular options include:

- **The ELK Stack (Elasticsearch, Logstash, Kibana):**

  - Elasticsearch: A scalable search engine for storing and indexing log data.
  - Logstash: A log collection and processing pipeline.
  - Kibana: A visualization interface for exploring and analyzing logs.

- **Splunk:** A powerful commercial platform for centralized log management with
  extensive analytics capabilities.

- **Graylog:** An open-source centralized log management platform that
  emphasizes search and compliance.

- **Cloud-based Logging Services:**
  - AWS CloudWatch Logs
  - Azure Monitor
  - Google Cloud Logging
