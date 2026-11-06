### Problem
When we want to store large files, we have two problems: where and how?
- **Where**: As the db can grow because of these storage requirements, we need to think about the query performance, the replication lag, the backup time and ofc course the storage.  If your blob size is more than *10MB* and contents do not need to be queried, better use an object storage like S3.
- **How**: Due to network bandwidth, we cannot send a big chunk of data, so we divide the chunks and then send the data to the server, now the server need to maintain the metadat of the chunk and store them in a place, chunks can be malicious, the delivery can fail or duplicate chunk can be sent. We need to support retries and resumability. 

### Solutions
1. Connect blob storage directly to the client and let it upload. This was we can support the retries and resumability.
	- When a client want to upload something, it asks the server for a pre-signed object storage URL, which is provided by the application by using its own credentials, no call the object storage happens here. 
	- This pre-signed URL contains permissions, the destination and the expiry. Client starts uploading on this URL.

	- Uploads
	![[Pasted image 20261013110356.png|912]]
	- Downloads:
	![[Pasted image 20261013111043.png|916]]
	- Note that CDNs are used for frequently accessed static resources. For infrequent downloads, we should prefer blob storage downloads.
	- Also, in case of partial uploads, which are stopped, we need to clean up the object  storage as it costs per file. So, we need to have a clean up job too.
	- **Resume job**: The client first creates the multipart upload and gets the upload ID and presigned part URLs, then uploads parts while recording their checksums. After reconnecting, it uses the upload ID with ListParts to find the missing parts instead of starting over. Once every part is in, it calls the completion endpoint so S3 assembles the final object.

#### Tradeoffs
- You are maintaining the file metadata on server's db and the actual file is in S3, we need to synchronise the state of the db and file upload. If we let client inform server about the upload completion => Problems:
	- Client might crash before this api call => orphaned file
	- Malicious client calling this api for no reason => malicious entry in the db.
- Solution: use object storage upload event + pending db entries, reconciliation job for failed events from object storage.

### Examples
- **Youtube** - Video uploads are the perfect use case. A user uploads a 2GB 4K video file. The web app generates a pre-signed S3 URL, the client uploads directly to S3 (often using multipart for resumability), and S3 events trigger transcoding workflows. Downloads work similarly - CloudFront serves video segments with signed URLs, enabling adaptive bitrate streaming without your servers ever touching the video bytes.

- **Instagram/Photo Sharing** - Photo uploads bypass the API servers entirely. The mobile app gets pre-signed URLs for original images (which can be 50MB+ from modern cameras), uploads directly to S3, then S3 events trigger async workers to generate thumbnails, apply filters, and extract metadata. The feed serves images through CloudFront with signed URLs to prevent hot-linking.

- **Dropbox/File Sync** - File uploads and downloads are pure serverless data access. When you drag a 500MB video into Dropbox, it gets pre-signed URLs for chunked uploads, uploads directly to blob storage, then triggers sync workflows. File shares generate time-limited signed URLs that work without the recipient having a Dropbox account.

- **WhatsApp/Chat Applications** - Media sharing (photos, videos, documents) uses pre-signed URLs. When you send a video in WhatsApp, it uploads directly to storage, then the chat system only passes around the file reference. Recipients get signed download URLs that expire after a period, ensuring media can't be accessed indefinitely.

##### When not to use
- No need to use for small files < 10MB JSON payload.
- When you need to validate the chunk in real time before storing.

### Common Questions
- If a malicious file is uploaded:
	- Use a quarantines bucket first, then run scans on it, after that move to the normal bucket.
	- *Tradeoff*: Latency for the file being ready.
	- Always have some limits of upload size in the pre-signed URL.
- 