# 🚀 ELEVATE_LABS_TASK_7
![AWS](https://img.shields.io/badge/AWS-AutoScaling-orange?logo=amazon-aws)
![EC2](https://img.shields.io/badge/EC2-Instances-blue?logo=amazon-aws)
![LoadBalancer](https://img.shields.io/badge/Application_Load_Balancer-green?logo=amazon-aws)
![CloudWatch](https://img.shields.io/badge/CloudWatch-Metrics-purple?logo=amazon-aws)
![GitHub](https://img.shields.io/badge/GitHub-Repo-success?logo=github)


**Task 7: Configure Load Balancing and Auto Scaling for a Web Application**

This project demonstrates a scalable AWS architecture using EC2, Application Load Balancer (ALB), and Auto Scaling Groups. It simulates real-world traffic, validates dynamic scaling, and showcases fault tolerance — all backed by CloudWatch metrics and GitHub documentation.

---

## 🧱 Architecture Overview

- **EC2 Instances**: Serve a static HTML page (“Hello from my Cloud Instance!”)
- **Application Load Balancer**: Distributes traffic across healthy instances
- **Auto Scaling Group**: Dynamically adjusts instance count based on CPU load
- **CloudWatch**: Monitors CPU utilization and triggers scaling actions

<img width="2244" height="752" alt="Screenshot 2025-10-31 121516" src="https://github.com/user-attachments/assets/78309928-2d10-49bf-b6cf-1270b54c9b18" />

---
## Auto Scaling TOPOLOGY MAP

<img width="795" height="556" alt="Screenshot 2025-10-31 114712" src="https://github.com/user-attachments/assets/51523288-cc4d-425b-9c9d-b4bc8948e3fa" />

---
## 📽️ Visual Demo: Auto Scaling in Action
This animation demonstrates how AWS Auto Scaling dynamically adjusts EC2 instances based on traffic load:

![Load Balancer Demo](https://eugeneyan.com/assets/load-balancer.gif)



1. **Initial State**: A single EC2 instance handles low traffic behind an Application Load Balancer (ALB).
2. **Traffic Spike**: Incoming requests increase, causing CPU utilization to rise above the scaling threshold.
3. **Scale-Out Triggered**: Auto Scaling Group (ASG) launches additional EC2 instances to distribute the load.
4. **Balanced Load**: With more instances, CPU usage stabilizes and performance improves.
5. **Traffic Drops**: As demand decreases, CPU utilization falls below the lower threshold.
6. **Scale-In Triggered**: ASG terminates extra EC2 instances to optimize cost and maintain efficiency.

This setup ensures high availability, cost-efficiency, and elasticity — key principles of cloud-native architecture.

> ✅ Verified using CloudWatch alarms, ASG activity logs, and stress-based CPU simulation.
---

## websites which arecreated using load balncer:
## 📽️ Auto Scaling Demo (GIF)

![Auto Scaling Demo](https://miro.medium.com/v2/resize:fit:1358/1*R_7DhgTEHQKoN5_B9EUzPg.gif)

- websites launched are:-
  <img width="2559" height="1319" alt="Screenshot 2025-10-31 110142" src="https://github.com/user-attachments/assets/42c45de0-d74d-4559-bcd0-1d4e2f6850ed" />



  <img width="816" height="149" alt="Screenshot 2025-10-31 115900" src="https://github.com/user-attachments/assets/57633f6b-dbc3-4d3b-88e7-e95803e70a17" />



*The above representsthe websites which are launched.*
---
## 🧗 Challenges Faced

- ⚠️ **Auto Scaling Delay**: Scaling actions took ~2 minutes to trigger after CPU spike. Required careful timing during traffic simulation.
- 🔍 **Health Check Failures**: Instances were marked unhealthy due to incorrect path configuration (`/index.html` missing). Fixed by updating Launch Template.
- 🔄 **Git Push Conflict**: Faced rejection when pushing local folder due to remote history. Resolved using `--allow-unrelated-histories`.
- 📊 **CloudWatch Graph Interpretation**: Matching CPU spikes with instance launch times required precise time zone alignment.
- 🧹 **Cleanup Validation**: Ensuring terminated instances were properly logged and removed from Load Balancer target group.

---
## 🧰 Real-Time AWS CLI Commands Used by Cloud Engineers

| Task | AWS CLI Command | Description |
|------|------------------|-------------|
| **Create Target Group** | `aws elbv2 create-target-group --name my-targets --protocol HTTP --port 80 --vpc-id vpc-xxxxxxxx` | Creates a target group for your ALB |
| **Create Load Balancer** | `aws elbv2 create-load-balancer --name my-alb --subnets subnet-aaa subnet-bbb --security-groups sg-xxxx` | Launches an ALB in specified subnets |
| **Register Targets** | `aws elbv2 register-targets --target-group-arn arn:aws:... --targets Id=i-xxxxxxxx` | Links EC2 instances to the target group |
| **Create Launch Template** | `aws ec2 create-launch-template --launch-template-name my-template --version-description v1 --launch-template-data file://template.json` | Defines EC2 instance config for Auto Scaling |
| **Create Auto Scaling Group** | `aws autoscaling create-auto-scaling-group --auto-scaling-group-name my-asg --launch-template LaunchTemplateName=my-template,Version=1 --min-size 1 --max-size 3 --desired-capacity 1 --vpc-zone-identifier subnet-aaa,subnet-bbb` | Sets up the Auto Scaling Group |
| **Attach Target Group to ASG** | `aws autoscaling attach-load-balancer-target-groups --auto-scaling-group-name my-asg --target-group-arns arn:aws:...` | Ensures ASG instances are routed through ALB |
| **Create Scaling Policy** | `aws autoscaling put-scaling-policy --auto-scaling-group-name my-asg --policy-name cpu-scale-out --policy-type TargetTrackingScaling --target-tracking-configuration file://policy.json` | Adds CPU-based scaling logic |
| **View Scaling Activities** | `aws autoscaling describe-scaling-activities --auto-scaling-group-name my-asg` | Shows instance launch/terminate logs |
| **Monitor CPU via CloudWatch** | `aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name CPUUtilization --dimensions Name=AutoScalingGroupName,Value=my-asg --start-time ... --end-time ... --period 300 --statistics Average` | Retrieves CPU metrics for scaling validation |

---
## 🛠️ Troubleshooting Commands Used by Cloud Engineers

| Task | Command | Description |
|------|---------|-------------|
| **Check Auto Scaling Activity** | `aws autoscaling describe-scaling-activities --auto-scaling-group-name my-asg` | Lists recent scaling events (launch/terminate) with timestamps and reasons |
| **Describe Auto Scaling Group** | `aws autoscaling describe-auto-scaling-groups --auto-scaling-group-names my-asg` | Verifies instance count, health status, and attached target groups |
| **Check Instance Health in Target Group** | `aws elbv2 describe-target-health --target-group-arn arn:aws:...` | Shows if instances are healthy/unhealthy and why |
| **List Registered Targets** | `aws elbv2 describe-target-groups --names my-targets` | Confirms which instances are registered with the Load Balancer |
| **Check Load Balancer DNS** | `aws elbv2 describe-load-balancers --names my-alb` | Retrieves the public DNS name of the ALB |
| **Check EC2 Instance Status** | `aws ec2 describe-instance-status --instance-ids i-xxxxxxxx` | Shows system and instance health checks |
| **View Launch Template Details** | `aws ec2 describe-launch-templates --launch-template-names my-template` | Confirms AMI, user data, and instance type |
| **Check CloudWatch Alarms** | `aws cloudwatch describe-alarms --alarm-names cpu-alarm` | Verifies if alarms are in ALARM/OK state |
| **Tail EC2 Logs (via SSH)** | `ssh -i key.pem ec2-user@<public-ip> 'tail -f /var/log/cloud-init.log'` | Live logs to debug instance boot issues |
| **Simulate Load** | `ab -n 500 -c 50 http://<alb-dns>` | Apache Benchmark to trigger CPU-based scaling |

---
## 🧠 Key Takeaways

- ✅ **Auto Scaling is reactive but predictable**: Scaling actions triggered within ~2 minutes of CPU breach, validating CloudWatch thresholds.
- ✅ **Health checks are critical**: Misconfigured paths (`/index.html`) caused instances to be marked unhealthy — fixed via Launch Template updates.
- ✅ **Load Balancer ensures fault tolerance**: Traffic was seamlessly routed even during instance termination, proving high availability.
- ✅ **Git push conflicts are solvable**: Learned to resolve remote history mismatches using `--allow-unrelated-histories`.
- ✅ **Apache Benchmark is a powerful test tool**: Simulated realistic load (`ab -n 500 -c 50`) to trigger scaling and validate performance.
- ✅ **Documentation matters**: Screenshots, timestamps, and diagrams helped communicate the setup clearly to recruiters and reviewers.
---

## 📡 CloudWatch Metrics & Traffic Flow Monitoring (Daily Use)

| Task | AWS CLI Command | Description |
|------|------------------|-------------|
| **Create Custom Metric** | `aws cloudwatch put-metric-data --namespace "Custom/WebApp" --metric-name PageHits --value 1 --dimensions "Page=Home"` | Pushes a custom metric (e.g., page hits) to CloudWatch |
| **List Available Metrics** | `aws cloudwatch list-metrics --namespace AWS/EC2` | Lists all EC2-related metrics (e.g., CPUUtilization, NetworkIn) |
| **Get Metric Statistics** | `aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name CPUUtilization --dimensions Name=InstanceId,Value=i-xxxxxxxx --start-time 2025-10-30T00:00:00Z --end-time 2025-10-31T00:00:00Z --period 300 --statistics Average` | Retrieves CPU usage data for a specific EC2 instance |
| **Describe Alarms** | `aws cloudwatch describe-alarms --alarm-names cpu-alarm` | Checks if alarms are in ALARM/OK state |
| **Check Load Balancer DNS** | `aws elbv2 describe-load-balancers --names my-alb` | Retrieves the ALB’s public DNS name for testing |
| **Check Target Health** | `aws elbv2 describe-target-health --target-group-arn arn:aws:...` | Shows health status of EC2 instances behind the ALB |
| **Check Auto Scaling Activity** | `aws autoscaling describe-scaling-activities --auto-scaling-group-name my-asg` | Lists recent scale-in/out events with timestamps |
| **Check EC2 Instance Status** | `aws ec2 describe-instance-status --instance-ids i-xxxxxxxx` | Verifies instance health and system checks |
| **Simulate Traffic Load** | `ab -n 500 -c 50 http://<alb-dns>` | Apache Benchmark to simulate concurrent traffic and trigger scaling |
| **Monitor Network Traffic** | `aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name NetworkIn --dimensions Name=InstanceId,Value=i-xxxxxxxx --start-time ... --end-time ... --period 300 --statistics Sum` | Tracks incoming traffic to EC2 instances |

---

## 📊 Why Use CloudWatch

**Amazon CloudWatch** is the backbone of monitoring and automation in this architecture. It tracks performance metrics, triggers scaling actions, and helps validate traffic flow across EC2 instances and Load Balancers.

---

### 🔍 CloudWatch vs Other AWS Monitoring Tools

| Feature | CloudWatch | CloudTrail | AWS Config |
|--------|------------|------------|-------------|
| **Purpose** | Performance & health monitoring | API call tracking | Resource configuration auditing |
| **Data Type** | Metrics, logs, alarms | Event history | Configuration snapshots |
| **Use Case** | Scaling, alerting, dashboards | Security analysis | Compliance & drift detection |

---

### ✅ Why CloudWatch Matters Here

- **Triggers Auto Scaling**: Monitors `CPUUtilization` and activates scale-out/in policies.
- **Validates Traffic Load**: Tracks `NetworkIn`, `RequestCount`, and `TargetResponseTime` to confirm scaling behavior.
- **Debugs Health Issues**: Helps identify why instances are marked unhealthy (e.g., failed health checks).
- **Supports Visual Documentation**: CloudWatch graphs are used to prove scaling events and performance trends.

---

### 📈 Example Metrics Used

- `CPUUtilization` → triggers scaling
- `NetworkIn` → confirms traffic flow
- `RequestCount` → measures ALB load
- `TargetResponseTime` → detects latency issues

---

## ⚙️ Setup Highlights

- Launch Template with user-data script to auto-deploy HTML
- Target Group attached to ALB for health checks
- Auto Scaling policy:
  - Scale out if CPU > 60%
  - Scale in if CPU < 30%
- Apache Benchmark used to simulate traffic:
  ```bash
  ab -n 500 -c 50 http://http://elevate-alb-378360910.us-east-1.elb.amazonaws.com/

