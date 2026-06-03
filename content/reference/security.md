---
title: Security
type: docs
bookToc: false
weight: 22
---

# Security

WebP Server Go applies path validation before reading local files or resolving remote URLs. Invalid, malformed, or traversal attempts are handled consistently.

## Path traversal protection

The server rejects directory traversal in request paths, including encoded variants:

| Attack pattern | Example | Result |
| -------------- | ------- | ------ |
| Plain traversal | `/../etc/passwd`, `..%2f` in path segments | `404 Not Found` |
| Single URL encoding | `%2e%2e%2f` | `404 Not Found` |
| Double URL encoding | `%252e%252e%252f` | `404 Not Found` |

Paths are decoded before segment checks so nested encodings cannot bypass validation.

Legitimate percent-encoded filenames (for example UTF-8 file names) remain supported when the resolved path stays within the configured base directory.

When `ALLOWED_TYPES` is set to `["*"]` (`AllowAllExtensions`), requests still go through the same safe path resolution; extension allow-all does not bypass traversal checks.

## Unified `404` responses

For security-sensitive and missing-resource cases, the server returns **`404 Not Found`** instead of mixing `400 Bad Request` or other codes. This includes:

* Malformed or non-standard request paths
* Path traversal or paths outside the allowed base
* Missing source files (local or remote)

Rationale: a single status code reduces information leakage to scanners (for example distinguishing “bad syntax” from “forbidden path”).

## Query parameters and metadata

The `meta=full` query parameter returns JSON metadata about a resource. It is only served after path validation and a successful source check. For illegal paths or missing files, the response is **`404`**, not cached or historical metadata. See [Query parameters](/reference/query-parameters/).
