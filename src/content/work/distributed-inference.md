---
title: Distributed Inference
summary: multi-vm llm serving on aws, provisioned with terraform
year: 2025
tags: [aws, terraform, devops, inference, infra-as-code]
repo: https://github.com/paramcodes/devops-infra
featured: true
order: 3
---

A distributed multi-VM inference pipeline built for Alchemyst AI on AWS. Model serving is spread across EC2 instances so throughput scales horizontally, with all instances locked inside private subnets — nothing serving traffic touches the public internet.

Everything is Terraform: the networking, the instances, the pipeline wiring. No console clicks, no snowflake infrastructure. The repo doubles as a working reference for running inference workloads behind proper VPC boundaries with infra-as-code from day one.
