# AWS Cloud Computing README (Basic)

This short README gives a beginner-friendly overview of key AWS services: EFS, EBS, S3, Lex, EC2, Triggers, SNS, and SQS, with what they are, when to use them, and simple notes to get started.

## Amazon EFS (Elastic File System)

- What it is: A serverless, elastic, shared file system that multiple EC2 instances can mount over NFSv4 within a VPC, growing and shrinking automatically with usage.[1][2]
- When to use: Shared storage for Linux workloads, web/app servers needing the same files across instances, content management, and data science pipelines.[2][1]
- Key points:
  - Concurrent access from many instances across multiple AZs in the same Region.[2]
  - Uses mount targets per AZ; mount via DNS that resolves to the local AZ endpoint.[2]
  - Fully managed; no capacity planning.[1]
- Getting started: Create an EFS file system, create mount targets, install NFS/EFS mount helper on EC2, mount and read/write files.[3][2]

## Amazon EBS (Elastic Block Store)

- What it is: Persistent block storage volumes for EC2, like virtual SSD/HDD disks, attached to a single instance at a time in the same AZ.[4][5]
- When to use: Boot volumes, databases, file systems on a single instance, transactional workloads needing low-latency block storage.[5][4]
- Key points:
  - Multiple volume types (SSD vs HDD) to optimize cost/performance; can resize and tune performance with Elastic Volumes.[4]
  - Snapshots for backup/restore and migration across accounts/Regions/AZs; supports encryption at rest and in transit between instance and volume.[4]
  - Data is replicated within an AZ for durability.[4]
- Getting started: Create a volume, attach to an EC2 instance in the same AZ, format/mount; volumes range from 1GB to 64TB.[5]

## Amazon S3 (Simple Storage Service)

- What it is: Object storage for any amount of data, organized as objects in buckets with features like versioning, lifecycle, and access controls.[6][7]
- When to use: Static assets, backups, logs, data lakes, media storage, and archival with multiple storage classes for cost optimization.[7][6]
- Key points:
  - Buckets store objects; object metadata includes key, version ID, and tags.[8]
  - Versioning preserves older versions (increases storage cost but protects against deletes/overwrites).[8]
  - Many storage classes exist (Standard, Intelligent-Tiering, IA, One Zone-IA, Glacier, Deep Archive) to match access patterns and cost.[9]
- Getting started: Create a bucket, upload objects, set bucket policies and optional versioning/lifecycle rules.[6]

## Amazon Lex

- What it is: Service to build conversational interfaces (chatbots/voice bots) using ASR and NLU, same tech behind Alexa.[10]
- When to use: Chatbots for customer support, booking, ordering, FAQ, and workflow automation across web/mobile/messaging platforms.[10]
- Key points:
  - Core concepts: Bot, Intents (user goals), Sample Utterances, Slots (parameters), Slot Types (built-in/custom).[10]
  - Integrates with AWS Lambda for validation and fulfillment; publish versions and aliases and deploy to channels.[10]
- Getting started: Create a bot, define intents and slots, test in console, publish and deploy.[11][10]

## Amazon EC2 (Elastic Compute Cloud)

- What it is: On-demand, scalable virtual servers (instances) with configurable instance types, networking, and storage in AWS.[12]
- When to use: General-purpose compute, web/app servers, batch processing, custom environments, and workloads not suited for serverless/containers.[12]
- Key points:
  - Choose instance types (CPU/memory/network/storage balance), AMIs, security groups, key pairs; attach EBS volumes.[12]
  - Default VPC/subnets provided in each Region; security groups act as virtual firewalls.[13]
  - Free Tier guidance and quick start tutorial available.[13]
- Getting started: Launch an instance from an AMI, create/select key pair and security group, connect (SSH/RDP), and clean up when done.[13]

## Triggers and Eventing (High-level)

- S3 bucket notifications can trigger workflows by sending events to SNS topics, SQS queues, or Lambda when objects are created/deleted.[14]
- SQS can trigger Lambda: Lambda polls queues and invokes functions to process messages, enabling serverless consumers.[15]
- SNS can fan out events to many subscribers, including SQS queues and Lambda, enabling pub/sub patterns.[16][17]

## Amazon SNS (Simple Notification Service)

- What it is: Managed pub/sub messaging; publishers send to topics, subscribers receive via protocols like HTTP/S, email, Lambda, or SQS.[17]
- When to use: Fan-out notifications, broadcast events to multiple systems, decoupling producers from consumers.[17]
- Key points:
  - Topics, subscriptions, and message attributes support filtering and routing; integrates with SQS for durable fan-out.[16][17]
  - Common pattern: publish once to SNS, deliver to multiple SQS queues for buffering and independent processing.[16]
- Getting started: Create a topic, add subscriptions, publish messages; optionally subscribe SQS queues with proper permissions.[16]

## Amazon SQS (Simple Queue Service)

- What it is: Fully managed message queuing for decoupling and buffering between microservices and components.[18]
- When to use: Smooth out traffic spikes, retry handling, asynchronous processing, and durability of messages with dead-letter queues.[18]
- Key points:
  - Works well with SNS for fan-out and with Lambda for serverless consumers; supports standard and FIFO queues.[15][18]
  - Recommended for critical processing to ensure near-guaranteed delivery when combined with SNS fan-out patterns.[19]
- Getting started: Create a queue, send/receive messages, optionally configure Lambda triggers or subscribe queue to an SNS topic.[15][16]

## Simple Comparisons

- EFS vs EBS vs S3:
  - EFS: Shared file system over NFS for multiple EC2s; POSIX semantics; elastic capacity.[1][2]
  - EBS: Block storage disk for a single EC2 in one AZ; low-latency; snapshots and encryption.[5][4]
  - S3: Object storage for any amount of data; bucket/object model; multiple storage classes and global-scale durability.[9][7]

## Quick Start Pointers

- EFS: Create file system → mount targets → mount on EC2 via EFS mount helper.[3][2]
- EBS: Create volume → attach to EC2 in same AZ → format/mount → use snapshots for backup.[5][4]
- S3: Create bucket → upload objects → configure policies/versioning/lifecycle.[7][6]
- Lex: Define bot/intents/slots → test → publish → deploy to channels → integrate Lambda for fulfillment.[11][10]
- EC2: Choose AMI/instance type → security group/key pair → launch and connect → attach EBS as needed.[12][13]
- Triggers: S3 notifications to SNS/SQS; SQS triggers Lambda; SNS fan-out to multiple subscribers.[14][17][15]
- SNS+SQS pattern: Use SNS to distribute events to multiple SQS queues for durability and throttled processing.[19][16]

Notes:
- Always consider networking (VPC/Subnets/Security Groups/IAM) and Region/AZ placement when connecting EC2, EFS, and EBS.[2][5][12]
- Choose S3 storage classes to match access patterns and cost goals; enable versioning prudently due to extra storage costs.[8][9]

Citations:[3][9][6][7][11][19][14][18][17][1][8][13][15][2][4][5][10][12][16]

[1] https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html
[2] https://docs.aws.amazon.com/efs/latest/ug/how-it-works.html
[3] https://docs.aws.amazon.com/efs/latest/ug/getting-started.html
[4] https://docs.aws.amazon.com/ebs/latest/userguide/what-is-ebs.html
[5] https://aws.amazon.com/ebs/getting-started/
[6] https://docs.aws.amazon.com/AmazonS3/latest/userguide/GetStartedWithS3.html
[7] https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html
[8] https://www.geeksforgeeks.org/introduction-to-aws-simple-storage-service-aws-s3/
[9] https://www.netapp.com/learn/aws-cvo-blg-s3-storage-the-complete-guide/
[10] https://docs.aws.amazon.com/lex/latest/dg/how-it-works.html
[11] https://aws.amazon.com/lex/getting-started/
[12] https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html
[13] https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html
[14] https://docs.aws.amazon.com/AmazonS3/latest/userguide/ways-to-add-notification-config-to-bucket.html
[15] https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-configure-lambda-function-trigger.html
[16] https://docs.aws.amazon.com/sns/latest/dg/subscribe-sqs-queue-to-sns-topic.html
[17] https://docs.aws.amazon.com/sns/latest/dg/welcome.html
[18] https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html
[19] https://www.jeremydaly.com/how-to-use-sns-and-sqs-to-distribute-and-throttle-events/
[20] https://www.geeksforgeeks.org/introduction-to-aws-elastic-file-systemefs/
[21] https://tutorialsdojo.com/amazon-efs/
[22] https://newsletter.simpleaws.dev/p/ebs-basics-best-practices
[23] https://aws.amazon.com/efs/getting-started/
[24] https://www.simplilearn.com/tutorials/aws-tutorial/aws-s3
[25] https://www.bmc.com/blogs/aws-efs-elastic-file-system/
[26] https://aws.amazon.com/efs/storage-classes/
[27] https://www.geeksforgeeks.org/devops/introduction-to-aws-elastic-block-storeebs/
[28] https://aws.amazon.com/efs/when-to-choose-efs/
[29] https://www.youtube.com/watch?v=bkagBEg5MrA
[30] https://www.w3schools.com/aws/aws_cloudessentials_awsebs.php
[31] https://www.geeksforgeeks.org/cloud-computing/what-is-amazon-lex/
[32] https://k21academy.com/ai-ml/amazon-lex/
[33] https://tutorialsdojo.com/amazon-lex/
[34] https://www.kommunicate.io/blog/amazon-lex-tutorial/
[35] https://ineuron.ai/course/amazon-lex-chatbot
[36] https://dev.to/aws-builders/ec2-101-understanding-the-basics-of-amazons-elastic-compute-cloud-2njb
[37] https://www.youtube.com/watch?v=4esqnMlMo8I
[38] https://www.w3schools.com/training/aws/amazon-ec2-basics.php
[39] https://www.geeksforgeeks.org/cloud-computing/what-is-elastic-compute-cloud-ec2/
[40] https://www.sumologic.com/glossary/aws-ec2/
