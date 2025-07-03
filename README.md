[User]
  |
  | 1. Sends a POST request with text content to /upload
  ↓
[API Gateway (HTTP API)]
  |
  | 2. Triggers the UploadHandler Lambda function
  ↓
[UploadHandler (Lambda)]
  |
  | 3. Uploads the content to S3 → `uploads/<uuid>.txt`
  | 4. Publishes a message to SNS topic with file metadata
  ↓
[SNS Topic: FileUploadTopic]
  |
  | 5. Forwards the message to SQS (subscribed)
  ↓
[SQS Queue: FileProcessingQueue]
  |
  | 6. Triggers ProcessHandler Lambda for each message
  ↓
[ProcessHandler (Lambda)]
  |
  | 7. Reads file from S3 (`uploads/`)
  | 8. Processes it (adds “PROCESSED” tag)
  | 9. Writes to S3 → `processed/<uuid>.txt`
  ↓
[Amazon S3: Processed Files]
  |
  | 10. Every hour (or manually), EventBridge triggers summary
  ↓
[EventBridge (Scheduled Rule)]
  |
  | 11. Invokes SummaryHandler Lambda
  ↓
[SummaryHandler (Lambda)]
  |
  | 12. Scans `processed/` in S3
  | 13. Writes file list to → `summary/summary.json`
  ↓
[S3: Summary File]
