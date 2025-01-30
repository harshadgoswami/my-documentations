To allow requests from **any domain** in the S3 CORS policy, simply set the `AllowedOrigins` to `"*"`.

### Example CORS Configuration for Any Domain:

```json
[
    {
        "AllowedHeaders": ["*"],
        "AllowedMethods": ["GET", "POST", "PUT", "DELETE", "HEAD"],
        "AllowedOrigins": ["*"],
        "ExposeHeaders": [],
        "MaxAgeSeconds": 3000
    }
]
```

### Explanation:

-   **AllowedHeaders:** `["*"]` allows any headers in requests.
-   **AllowedMethods:** Defines the HTTP methods allowed (you can adjust as needed).
-   **AllowedOrigins:** `["*"]` allows requests from any domain.
-   **ExposeHeaders:** Lists headers that can be exposed in the response. It's empty here.
-   **MaxAgeSeconds:** Defines how long the browser can cache the preflight request.

### Setting CORS Policy with AWS CLI:

1. Save the configuration to a file (`cors.json`).

    Example command:

    ```bash
    aws s3api put-bucket-cors --bucket <bucket-name> --cors-configuration file://cors.json
    ```

### Important Note:

Using `["*"]` in `AllowedOrigins` allows requests from **any domain**, which may not be secure for sensitive data. Use it only when necessary.
