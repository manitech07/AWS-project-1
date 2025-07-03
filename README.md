# 🚀 Serverless File Processing Pipeline on AWS

This project demonstrates a real-world serverless architecture using:

✅ AWS Lambda  
✅ API Gateway  
✅ S3  
✅ SNS  
✅ SQS  
✅ EventBridge  
✅ CloudFormation (IAC)

---

## 📘 Project Overview

A user submits a text file via a public API. That content is stored in S3 and triggers a processing pipeline using AWS messaging services.

### 🔁 Workflow

1. **User** submits text to API Gateway.
2. **Lambda (UploadHandler)** stores the file in `uploads/` and sends a message via SNS.
3. **SNS** publishes to an **SQS queue**.
4. **Lambda (ProcessHandler)** receives the message from the queue, processes the file (adds metadata), and stores the result in `processed/`.
5. Every hour, **EventBridge** triggers **SummaryHandler** to scan processed files and generate a `summary/summary.json`.

---

## 🔧 Technologies Used

| Service         | Purpose                                     |
|-----------------|---------------------------------------------|
| **API Gateway** | Public endpoint to submit file content      |
| **Lambda**      | Serverless functions for upload & processing|
| **S3**          | File storage                                |
| **SNS/SQS**     | Messaging & async event triggers            |
| **EventBridge** | Scheduling summary reports                  |
| **CloudFormation** | Infrastructure as Code (IaC)             |

---

## 🚀 Deploy and Test Instructions

```bash
aws cloudformation deploy \
  --template-file file-pipeline.yaml \
  --stack-name FilePipelineDemo \
  --capabilities CAPABILITY_NAMED_IAM

curl -X POST https://<your-api-endpoint>/upload \
  -d '{"content":"Hello from Mani!"}' \
  -H "Content-Type: application/json"
```

## 👤 Author

<p align="left">
  <a href="https://github.com/<your-github-username>" target="_blank">
    <img src="https://avatars.githubusercontent.com/<your-github-username>" width="100" style="border-radius: 50%;" />
  </a>
</p>

**Manikanta Gunda**  
☁️ Cloud and DevOps Engineer  
🚀 Passionate about AWS, DevOps, and Automation  


