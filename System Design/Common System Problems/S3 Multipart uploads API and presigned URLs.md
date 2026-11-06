### Pre-signed URL
Let's you do one action on one object without the credentials. Server creates this URL by itself, not calling S3, server uses its own credentials.
Takes:
- Operation: GET/PUT
- Expiry
- Chunk checksum, content type
- exact object key
- Content length can be specified with a POST or after the file completes uploading.

### How it works for client-server uploads
- Client asks server to upload a file.
- Server processes the request, calls CreateMultiPartUpload and gets the upload_id, saves in the db with this and status: uploading.
- For each part, server created pre-signed URL.
- Client gets these and upload. 
- Once done, client can send a done or server can get a notification from the S3 on file upload and it will call CompletePartUpload -> marks table row as done.

**On resume:** the client asks the server. The server calls `ListParts`, returns fresh presigned URLs for the missing parts only.
**Note:** the client never holds AWS credentials. It only gets per-part signed URLs.


### Common Questions
**“How does the server know the upload finished?”**  
Either the client calls “complete,” or an **S3 event notification** (→ SQS/Lambda) tells you. The event is more reliable, since the client can disappear.

**“What if the client never calls complete?”**  
The DB record stays `uploading`. A cleanup job aborts it after expiry, plus the lifecycle rule as a backup.

**“How do you prevent someone uploading a huge file?”**  
Presigned POST with a size policy, or a declared size checked at the start plus validation after upload.

**“How do you verify integrity?”**  
Send a checksum with each part (`Content-MD5` or the newer SHA-256 checksums). S3 rejects the part if it doesn’t match.

**“How are downloads handled?”**  
Presigned GET URLs, or signed **CDN** URLs for frequently read content. Range requests let clients download parts in parallel or resume.

**“Multipart vs your own chunking?”**  
Multipart = one big object, good for large media. Your own content chunks = many small objects, good for sync, dedup, and edits (Dropbox). Small chunks (~4 MB) don’t need multipart at all.