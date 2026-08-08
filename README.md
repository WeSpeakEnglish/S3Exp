# S3Exp is a S3 Browser

A self-contained, single-file S3 browser that runs entirely in your browser - no server, no install, no dependencies to manage. Just open the HTML file and start browsing.

---

## Features

- **Multi-bucket support** — save and switch between multiple connections in one click
- **S3-compatible services** — works with AWS S3, MinIO, Cloudflare R2, Wasabi, DigitalOcean Spaces, and any S3-compatible endpoint
- **File operations** — upload (with progress), download, delete single or multiple files
- **Folder navigation** — browse prefixes with breadcrumb path bar, create folders
- **Presigned share URLs** — generate temporary download links with configurable expiry (15 min → 7 days)
- **Drag & drop upload** — drop files anywhere on the window
- **Sort & filter** — sort by name / size / date, live filter by filename
- **Secure by design** — credentials live in `sessionStorage` only, never written to disk or localStorage; cleared automatically when the tab is closed

---

## Quick Start

1. Open `s3exp.html` in any modern browser (Chrome, Firefox, Edge, Safari)
2. Click **+ Add** in the sidebar
3. Fill in your credentials and bucket name, then click **Connect**

> **HTTPS required for remote use.** If you host the file on a web server, serve it over HTTPS. Running it locally via `file://` or `localhost` is fine.

---

## CORS Configuration

Browser-based S3 access requires CORS to be enabled on your bucket. In the AWS Console:

**S3 → Your Bucket → Permissions → Cross-origin resource sharing (CORS)**

Paste this configuration:

```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET", "PUT", "POST", "DELETE", "HEAD"],
    "AllowedOrigins": ["*"],
    "ExposeHeaders": ["ETag"]
  }
]
```

For production use, replace `"*"` in `AllowedOrigins` with the specific origin you serve the file from.

---

## Minimum IAM Permissions

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket",
        "s3:GetObject",
        "s3:PutObject",
        "s3:DeleteObject"
      ],
      "Resource": [
        "arn:aws:s3:::your-bucket-name",
        "arn:aws:s3:::your-bucket-name/*"
      ]
    }
  ]
}
```

---

## Multi-Bucket Connections

Each saved connection stores:

| Field | Description |
|---|---|
| Label | A friendly name, e.g. "Production", "Staging" |
| Access Key ID | AWS access key |
| Secret Access Key | AWS secret key |
| Region | e.g. `us-east-1` |
| Bucket Name | The S3 bucket to browse |
| Custom Endpoint | Optional — for non-AWS providers |

Connections are saved for the duration of the browser session. Click any entry in the sidebar to switch buckets instantly. Hover an entry and click **✕** to remove it.

---

## S3-Compatible Services

| Service | Endpoint format |
|---|---|
| Cloudflare R2 | `https://<account-id>.r2.cloudflarestorage.com` |
| MinIO | `http://your-minio-host:9000` |
| Wasabi | `https://s3.<region>.wasabisys.com` |
| DigitalOcean Spaces | `https://<region>.digitaloceanspaces.com` |
| Backblaze B2 | `https://s3.<region>.backblazeb2.com` |
| STORJ | https://gateway.storjshare.io |  

And others. Enter the endpoint URL in the **Custom Endpoint** field when adding a connection. Path-style requests are used automatically.

---

## Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl / ⌘ A` | Select all items |
| `Delete` | Delete selected items |
| `F5` | Refresh current folder |
| `Escape` | Close open dialogs |

---

## Security Notes

- Credentials are held **only in `sessionStorage`** - they are never written to `localStorage`, cookies, or disk
- Closing or refreshing the tab clears all credentials immediately
- You can also click **Clear Session** at any time to wipe everything manually
- Presigned URLs are generated client-side using the AWS SDK and do not pass through any third-party server

---

## Technical Details

- **Single HTML file** - AWS SDK (`aws-sdk-2.x`), styles, and all logic are bundled in one file
- **No build step** - no Node.js, npm, or bundler required
- **AWS SDK v2** - loaded from the official AWS CDN
- **Font** - Inter (UI) + JetBrains Mono (monospace) via Google Fonts

---

## Browser Support

Any modern browser released after 2020. JavaScript must be enabled.

---

## License

MIT — do whatever you want with it.
