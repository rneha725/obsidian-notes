- [[[Pattern] Handling Large Blobs]]
- [[S3 Multipart uploads API and presigned URLs]]
- We store the chunks in the S3 bucket, so if there are some changes, we can upload the changed chunk.
	- Client creates hashes of chunk and asks server if they have anything different, if yes then S3 stores the new chunk as a new version.
- While designing dropbox, I was confused why not use a manual pull mechanism it is very simple. In interviews I can ask what is the ask around the users getting to know about the new change.
	- If we are fine user finding out -> manual pull is fine. Every device of that user needs to pull and people might forget. 
		- Shared document edits could pose an bigger conflicts but we can always remember to pull the file when we open or edit?
		- A manual pull might result into bigger file chunks getting downloaded.
		- Good for: git, rarely changing data.
	- Short polling interval matters a lot.
	- Dropbox uses: 
		- Push notifications through web sockets and given this notification a user can pull the data.
		- It also pulls the data with long intervals.
		- How Web Sockets work in this context:
			- So we have notification servers, which are connected to online devices.
			- We have a device db, which tells us for this user id, what all devices are registered.
			- We store the device: service info in redis so we can send the request to the right notification servers when a user want to sent the sync notification.
- Why use chunking?
	- A big file, if failed then we need to restart the upload.
	- We can also show the user a progress bar.
	- Limit of object storage, a single put can be 5GB.
	- Parallel uploads.
	- Cheap retries.
- When we create chunks we also need to take care of resumability of the chunks,
	- We need fingerprints of the file, the chunks on the client side.
	- The chunk info is also stored in the file metadata table, with the status, updated only when the client sends the request saying it has uploaded.
	- We can check with the S3 if all the chunks are uploaded successfully. Client combines these chunks.
	- S3 do not create a notification for the chunk upload only for the all chunks uploaded, 

### File upload flow:
1. **Client** splits the file, hashes every chunk, saves the hash list locally.
2. **Client → server:** “I want to upload this file. Here are its chunk hashes.”
3. **Server** creates an upload session and checks which hashes already exist. It replies with the missing ones plus a presigned URL for each.
4. **Client** uploads missing chunks straight to S3, often in parallel. It marks each chunk done locally after success.
5. **Server learns a chunk arrived**, either from the client (“chunk h3 done”) or from an S3 event. It marks `h3 = stored`, after checking the hash matches.
6. When all chunks are stored, **client → server:** “Commit.”
7. **Server** writes the new manifest and version in one transaction, and emits the change event.


### What happens on a crash
Say the laptop dies after uploading 60 of 100 chunks.

1. **Client restarts** and reads its local DB: there’s an unfinished session.
2. **Client → server:** “Session S, which chunks are you missing?”
3. **Server** checks its `chunks` table and returns the 40 missing hashes, with fresh presigned URLs (the old ones may have expired).
4. **Client** uploads only those 40, then commits.

### How did we update and upload?
We have chunks on the client side, when an update happens, it shifts all the chunks after it if we take hash like this. For this to resolve, we use something called CDC.
This problem does not come up when we overwrite.
The fix: content-defined chunking (CDC): Use a rolling hash.