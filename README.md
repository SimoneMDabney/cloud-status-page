# Cloud Status Page

A small monitoring system that checks whether websites and ports are up, logs the results, and publishes them to a public status page hosted on AWS.

> **Status:** 🚧 In progress (currently at Stage 1)

## Why I built this

<!-- 2-3 sentences: what you wanted to practice (Linux, Bash, Git, networking, Python, AWS) and why a status page was a good way to do it. -->

## Architecture

<!-- Add your diagram from diagrams.net here, e.g. ![Architecture](docs/architecture.png) -->

**Flow:**
1. Cron runs the Bash wrapper on a schedule
2. The wrapper runs the Python checker
3. The checker reads `config.yaml`, tests each target, and writes `results.json`
4. The results are uploaded to S3
5. The S3 static site displays the latest results

## Tech used

| Area | Tools |
|---|---|
| Scripting | Bash, Python 3 |
| Config / data | YAML, JSON |
| Scheduling | cron |
| Version control | Git, GitHub |
| Cloud | AWS (VPC, EC2, IAM, S3) |

## Project structure

```
cloud-status-page/
├── config.yaml        # targets and settings
├── checker.py         # Python health checker (Stage 2)
├── run_checker.sh     # Bash wrapper + log rotation (Stage 1)
├── index.html         # status page shown from S3 (Stage 3)
├── docs/              # diagrams and screenshots
└── README.md
```

## Setup

### Prerequisites
- Linux (VM, WSL2, or EC2)
- Python 3.x
- Git
- An AWS account with a billing alert enabled (for Stage 3)

### Local run
```bash
# TODO: fill in as you build
git clone <your-repo-url>
cd cloud-status-page
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python3 checker.py
```

## Configuration

Targets are defined in `config.yaml`:
- `http_targets`: URL plus the expected status code
- `tcp_targets`: host plus port
- `settings`: timeout, output file, log file, and log retention

## Results format

<!-- Paste a short example of your results.json once your checker works. Each entry should include: target, status, response time, timestamp. -->

## AWS deployment (Stage 3)

<!-- Document what you built and the settings you chose: -->
- **VPC / subnet:** 
- **EC2 instance type:** 
- **Security group rules:** (SSH only from my IP)
- **IAM role permissions:** (write access to one S3 bucket only)
- **S3 bucket / static site setup:** 

## Security and cost notes

- Uses an IAM role on the instance, no access keys stored anywhere
- Security group restricts SSH to my IP only
- Billing alert set before launching resources
- Stays within the AWS Free Tier
- All resources torn down after testing (verified in the billing dashboard)

## Progress checklist

- [ ] **Stage 1:** Bash wrapper, cron job, log rotation, repo on GitHub
- [ ] **Stage 2:** Python checker with YAML config, JSON output, error handling
- [ ] **Stage 3:** VPC, EC2, IAM role, S3 status page, scheduled uploads
- [ ] **Stage 4:** Cleanup, README, architecture diagram

## What I learned

<!-- Fill in at the end: what was hardest, what broke, how you debugged it, what you'd do differently. This section is what employers actually read. -->

## Stretch goals

- [ ] CloudWatch alarm or SNS email when a target goes down
- [ ] Rebuild the infrastructure in Terraform
- [ ] GitHub Actions workflow to lint Python and Bash on every push
- [ ] Private subnet plus NAT design with a cost-tradeoff note
