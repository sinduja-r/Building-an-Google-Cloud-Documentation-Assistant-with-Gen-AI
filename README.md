# Building-an-AWS-Documentation-Assistant-with-Gen-AI
An Interactive assistant that fetches and summarizes official AWS documentation for any AWS service

## Use Case:
Many developers and cloud engineers face challenges navigating AWS documentation to troubleshoot or learn how to use a service. The documentation is vast, fragmented across many pages, and often requires specific search skills to get the right answer quickly.

## The Idea: **An AI-Powered AWS Documentation Assistant**
This project leverages Generative AI + Web Search Integration to simplify AWS troubleshooting. Just type a natural language question like "How to create an EC2 instance?", and the assistant will:
  - Search AWS docs using SerpAPI + Google
  - Extract structured, step-by-step content
  - Format and present a concise, human-readable guide


## How It Works: **Gen AI Under the Hood**

### 1. Retrieval-Augmented Generation (RAG)
Instead of hallucinating answers, the system:
- Searches **live AWS docs** using SerpAPI (Google Search API)
- Extracts **relevant sections** (e.g., headings, steps, code blocks)
- Generates **concise summaries** with clear formatting

### 2. Document Understanding
AWS documentation uses complex HTML formats. This project uses:
- `BeautifulSoup` to parse `<h1>`, `<h2>`, `<ol>`, `<p>` etc.
- `Regex` and light NLP to clean formatting artifacts (e.g., weird unicode symbols)

### 3. Structured Output Generation
No more walls of raw text! The assistant formats answers into:
- Numbered steps
- Section headers
- Bullet points and clean formatting

## Tech Stack & Libraries

| Tool/Library            | Purpose                                                                 |
|-------------------------|-------------------------------------------------------------------------|
| `requests`              | API calls to SerpAPI and to fetch HTML from AWS docs                    |
| `BeautifulSoup (bs4)`   | Parse and extract structured documentation sections                     |
| `re`                    | Clean up noisy formatting and unicode                                   |
| `kaggle_secrets`        | Secure access to SerpAPI key in Kaggle environments                     |



##  Example Usage 1: 

AWS Service Assistant (type 'quit' to exit)
Examples: 'How to setup S3 bucket', 'EC2 connection troubleshooting'

What AWS service do you need help with?  **What are the Storage options available**

Searching AWS documentation for: What are the Storage options available
Found 3 relevant resources. Processing...
Extracting content from: https://aws.amazon.com/products/storage/
Extracting content from: https://docs.aws.amazon.com/whitepapers/latest/aws-overview/storage-services.html


 AWS WHAT ARE THE STORAGE OPTIONS AVAILABLE GUIDE 
 ==================================================
            
Storage:
- AWS provides a broad portfolio of storage services with deep functionality for storing, accessing, 
  protecting, and analyzing your data.
- Each service is described after the diagram. To help you decide which service best meets your
  needs, see Choosing an AWS storage service . For general information, see Cloud Storage on AWS .

AWS Backup:
- AWS Backup enables you to centralize and automate
   data protection across AWS services. AWS Backup offers a cost-effective, fully managed, policy-based
   service that further simplifies data protection at scale. AWS Backup also helps you support your
   regulatory compliance or business policies for data protection. Together with AWS Organizations, AWS Backup
   enables you to centrally deploy data protection policies to configure, manage, and govern your
   backup activity across your organizationâs AWS accounts and resources, including Amazon Elastic Compute Cloud
   (Amazon EC2) instances, Amazon Elastic Block Store (Amazon EBS) volumes, Amazon Relational Database Service (Amazon RDS) databases (including Amazon Aurora
   clusters), Amazon DynamoDB tables, Amazon Elastic File System (Amazon EFS) file systems, Amazon FSx for Lustre file systems,
   Amazon FSx for Windows File Server file systems, and AWS Storage Gateway volumes.

Amazon Elastic Block Store:
- Amazon Elastic Block Store (Amazon EBS) provides persistent block storage
   volumes for use with Amazon EC2 instances in the AWS Cloud. Each Amazon EBS volume is automatically
   replicated within its Availability Zone to protect you from component failure, offering high
   availability and durability. Amazon EBS volumes offer the consistent and low-latency performance
   needed to run your workloads. With Amazon EBS, you can scale your usage up or down within minutes--all
   while paying a low price for only what you provision.

AWS Elastic Disaster Recovery:
- AWS Elastic Disaster Recovery (Elastic Disaster Recovery) minimizes downtime and
   data loss with fast, reliable recovery of on-premises and cloud-based applications using
   affordable storage, minimal compute, and point-in-time recovery. You can configure replication
   and launch settings, monitor data replication, and launch instances for drills or recovery.
- Set up Elastic Disaster Recovery on your source servers to initiate secure data replication. Your data is
   replicated to a staging area subnet in your AWS account, in the AWS Region that you select.
   You can perform non-disruptive tests to confirm that implementation is complete. During normal
   operation, maintain readiness by monitoring replication and periodically performing
   non-disruptive recovery and failback drills.
- If you must replicate to the AWS China Regions or perform replication and recovery into
   AWS Outposts, use CloudEndure Disaster Recovery available in the AWS Marketplace.

Amazon Elastic File System:
- Amazon Elastic File System (Amazon EFS) provides a simple, scalable, elastic
     
 ================================================== 
 # Check the links provided above for more details


##  Example Usage 2: 
What AWS service do you need help with? ** Troubleshooting steps for IAM permission issue in EC2 instance**

Searching AWS documentation for: Troubleshooting steps for IAM permission issue in EC2 instance
Found 3 relevant resources. Processing...
Extracting content from: https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_iam-ec2.html
Extracting content from: https://aws.amazon.com/premiumsupport/knowledge-center/kms-iam-ec2-permission/


 AWS TROUBLESHOOTING STEPS FOR IAM PERMISSION ISSUE IN EC2 INSTANCE GUIDE 
 ==================================================
            
Troubleshoot IAM and Amazon EC2:
- The following information can help you troubleshoot IAM issues with Amazon EC2.
- When I try to launch an instance, I don't
        see the role in the Amazon EC2 console IAM Role list
- When I attempt to call the
          AddRoleToInstanceProfile, I get an AccessDenied error
- Amazon EC2: When I try to launch an
        instance with a role, I get an AccessDenied error
- I can't access the temporary security
        credentials on my EC2 instance
- What do the errors from the
          info document in the IAM subtree mean?

When I try to launch an instance, I don't
        see the role in the Amazon EC2 console IAM Role list:
- If you are signed in as an IAM user, verify that you have permission to call ListInstanceProfiles . For information about the permissions necessary to
          work with roles, see Permissions required for using roles
        with Amazon EC2 . For information about adding
          permissions to a user, see Manage IAM policies .
- If you cannot modify your own permissions, you must contact an administrator who can
          work with IAM in order to update your permissions.
- If you created a role using the IAM CLI or API, verify the following:
- You created an instance profile and added the role to that instance
              profile.
- You used the same name for the role and the instance profile. If you name your
              role and instance profile differently, you won't see the correct role name in the
              Amazon EC2 console.
- The IAM Role list in the Amazon EC2 console lists the names of
          instance profiles, not the names of roles. You will have to select the name of the
          instance profile that contains the role you want. For details about instance profiles, see Use instance profiles .
- If you use the IAM console to create roles, you don't need to work with instance
            profiles. For each role that you create in the IAM console, an instance profile is
            created with the same name as the role, and the role is automatically added to that
            instance profile. An instance profile can contain only one IAM role, and that limit
            cannot be increased.

The credentials on my instance are for the
        wrong role:
- The role in the instance profile might have been replaced recently. If so, your
      application will need to wait for the next automatically scheduled credential rotation before
      credentials for your role become available.
- To force the change, you must disassociate the instance
        profile and then associate the instance profile , or you can stop your instance and then restart
      it.

When I attempt to call the
          AddRoleToInstanceProfile, I get an AccessDenied error:
- If you are making requests as an IAM user, verify that you have the following
      permissions:
- iam:AddRoleToInstanceProfile with the resource matching the instance
          profile ARN (for example, arn:aws:iam::9  
 ================================================== 
 # Check the links provided above for more details

 
