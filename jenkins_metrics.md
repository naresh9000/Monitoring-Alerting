#  Jenkins Metrics:

Build Execution Time:

Monitor the time taken for Jenkins builds to execute.
Example query: jenkins_job_execution_time_seconds{job="job_name"}
Build Success/Failure Rate:

Monitor the success and failure rates of Jenkins builds.
Example query: jenkins_job_builds{job="job_name", result="SUCCESS"}
Queue Length:

Monitor the length of the Jenkins build queue.
Example query: jenkins_queue_length
Executor Status:

Monitor the status of Jenkins executors (e.g., idle, busy).
Example query: jenkins_executors_status

Build Queue Wait Time:

Monitor the time jobs spend waiting in the Jenkins build queue before execution.
Example query: jenkins_queue_wait_time_seconds
Build Duration Trend:

Monitor the trend of build durations over time to identify performance changes.
Example query: jenkins_job_build_duration_seconds{job="job_name"}
Build Success/Failure Rate Trend:

Monitor the trend of build success and failure rates over time.
Example query: sum(rate(jenkins_job_builds{job="job_name", result="SUCCESS"}[5m])) (for success rate) and similar query for failure rate
Builds in Progress:

Monitor the number of builds currently in progress in Jenkins.
Example query: jenkins_builds_in_progress
Job Failure Rate:

Monitor the rate of job failures across different jobs.
Example query: rate(jenkins_job_failed_builds{job="job_name"}[5m])
Job Success Rate:

Monitor the rate of job successes across different jobs.
Example query: rate(jenkins_job_successful_builds{job="job_name"}[5m])
Build Health Trends:

Monitor the trend of build health over time to identify patterns.
Example query: jenkins_job_build_health_score{job="job_name"}
Agent Utilization:

Monitor the utilization of Jenkins agents (executors) over time.
Example query: jenkins_agent_utilization{agent="agent_name"}
Plugin Metrics:

Monitor metrics related to specific Jenkins plugins (e.g., Pipeline, Git, Docker).
Example query: jenkins_plugin_metrics{plugin="plugin_name"}
Failed Tests Trend:

Monitor the trend of failed tests across different builds and jobs.
Example query: sum(rate(jenkins_job_failed_tests{job="job_name"}[5m]))