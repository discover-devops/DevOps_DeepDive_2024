### **Day 5: Continuous Monitoring & Infrastructure Provisioning**

#### **Hour 2: Monitoring Infrastructure Health with ELK**

---

#### **Concepts: Analyzing Logs and Monitoring Performance Metrics**

Once the **ELK Stack** is set up, the next step is to use it effectively to **monitor infrastructure health** and analyze logs for performance metrics. Continuous monitoring of logs provides real-time insights into the health of the system, helping you detect issues like resource bottlenecks, latency spikes, or error patterns early on.

**Key Metrics to Monitor**:
- **System Performance**: CPU, memory, disk usage, and network bandwidth logs can help identify performance bottlenecks.
- **Application Performance**: Application logs can be analyzed for errors, response times, and load patterns to detect issues affecting users.
- **Availability and Uptime**: Logs can reveal system crashes, service outages, or downtimes.
- **Security Events**: Track security-related events such as unauthorized access attempts, failed logins, or suspicious activities.

---

**Using Kibana for Log Analysis**:
- **Dashboards**: Kibana allows you to create custom dashboards for visualizing log data in real-time.
- **Alerts**: Set up alerts to notify teams when certain thresholds or error conditions are met.
- **Search and Filter**: Kibana’s powerful search functionality allows you to filter logs based on specific fields or keywords.
- **Visualizations**: Create charts, graphs, and tables to understand performance metrics, error rates, and resource utilization.

---

#### **Explaining the Concept Using Real-World Use Cases**

1. **Real-World Use Case: Detecting High CPU Usage in Production Systems**:
   - A company uses Kibana to monitor CPU usage across its production servers. A sudden spike in CPU logs is detected, triggering an alert. The DevOps team investigates and finds that an inefficient query is overloading the database, enabling them to fix the issue before it affects users.

2. **Real-World Use Case: Monitoring Application Latency**:
   - An e-commerce platform collects logs from its application servers, which include response times for each request. By visualizing these logs in Kibana, the team detects a gradual increase in latency during peak hours, prompting them to scale up resources to handle the increased load.

---

#### **Hands-on Lab: Analyze Logs and Infrastructure Health Using Kibana Dashboards**

In this lab, participants will analyze system and application logs using **Kibana** dashboards. They will create custom visualizations to monitor key infrastructure metrics and set up alerts for critical performance issues.

---

### **Lab 1: Analyzing System and Application Logs in Kibana**

**Prerequisites**:
- A running **ELK Stack** from the previous lab.
- Logs being ingested into Elasticsearch (system logs, application logs, etc.).

---

**Step 1: Create a Kibana Dashboard for System Health Monitoring**

1. **Log in to Kibana**:
   - Open your browser and go to the Kibana interface (`http://localhost:5601`).

2. **Create an Index Pattern**:
   - In the Kibana interface, navigate to **Management** → **Index Patterns**.
   - Create an index pattern that matches the logs stored in Elasticsearch (e.g., `logstash-*`).

3. **Create Visualizations**:
   - Go to **Visualize** in the Kibana dashboard and start creating visualizations for the following system metrics:
     - **CPU Usage**: Create a line chart to monitor CPU usage over time by analyzing `syslog` or custom logs that report CPU metrics.
     - **Memory Usage**: Use a bar graph to visualize memory usage on the server. Filter the logs to extract memory-related fields.
     - **Disk I/O**: Create a gauge to show disk read/write activity by analyzing system logs.

4. **Create a Dashboard**:
   - After creating individual visualizations, combine them into a single dashboard.
   - Go to **Dashboard** → **Create New Dashboard** and add all the visualizations you created.

---

**Step 2: Analyzing Application Logs for Error Rates**

1. **Filter Application Logs**:
   - In the **Discover** section of Kibana, search for your application logs (e.g., logs ingested from `/var/log/myapp.log`).
   - Use search queries to filter out specific error messages:
     ```bash
     level: "error"
     ```

2. **Create a Visualization for Error Rates**:
   - Use a **vertical bar chart** to count the number of errors per time interval (e.g., by hour or minute).
   - Set the **Y-axis** to count the number of logs and the **X-axis** to the time field (`@timestamp`).

3. **Add Error Rate Visualization to the Dashboard**:
   - Add the error rate chart to the system monitoring dashboard created earlier.
   - Now your dashboard will show both system health metrics and application error rates in a unified view.

---

**Step 3: Set Up Alerts for Critical Issues**

1. **Install and Configure Alerting Plugin** (if not already installed):
   - Kibana’s alerting system allows you to set up notifications for critical events. Ensure the alerting feature is available and configured on your Kibana installation.

2. **Create an Alert for High CPU Usage**:
   - Go to **Management** → **Alerts and Actions**.
   - Create an alert that triggers when CPU usage exceeds a certain threshold.
   - Define the conditions (e.g., if CPU usage exceeds 80% for more than 5 minutes).
   - Configure the alert to send an email or Slack notification to the DevOps team.

3. **Create an Alert for Application Errors**:
   - Set up another alert for when the error rate in the application logs exceeds a certain number (e.g., more than 10 errors in 5 minutes).
   - This will help teams quickly identify issues before they impact users.

---

**Step 4: Monitor Infrastructure Health**

1. **View the Dashboard**:
   - Regularly monitor the Kibana dashboard to keep track of system and application health.
   - Use the dashboard to proactively identify issues like CPU spikes, memory exhaustion, or application errors.

2. **React to Alerts**:
   - When an alert is triggered (e.g., high CPU usage or application errors), the team should investigate the root cause by drilling into the logs using Kibana’s search and filter tools.

---

### **Lab Wrap-Up**

By the end of this lab, participants will have:
- Created a **Kibana dashboard** to monitor infrastructure health, including CPU, memory, and disk I/O metrics.
- Filtered and analyzed **application logs** for error rates and performance issues.
- Set up **alerts** to notify the team when critical thresholds (e.g., high CPU usage or error rates) are met.

---

This lab gives participants hands-on experience in using Kibana to monitor the health of their infrastructure and applications in real-time. 
