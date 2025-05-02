## What Is a Software Attestation?

- **Definition:** An authenticated statement (often in JSON or in-toto format) describing metadata about how an artifact was produced:
    
    - Source‐control commit ID
        
    - Build environment (compiler version, OS, container image)
        
    - Build steps and parameters
        
    - Provenance chain (who triggered the build, when)
        
- **Purpose:** Enables automated policy engines to enforce, for example, “Only deploy if built by our approved CI system, from the `main` branch, using Alpine Linux 3.18.”

## Base64 encoded attestation statement sample

```json
{
  "payloadType": "application/vnd.in-toto+json",
  "payload": "eyJzdWJqZWN0IjpbeyJuYW1lIjoiYnVpbGRkZXIucGFnZSIsInNoYTEiOiIyMzQ1ZjY4MTQ1ZjhiZTQwYjFlNmJiZjExMTEyZjE1NTM2ZDQ3ODMzIn1dLCJwcmVkaWNhdGVEYXRhIjp7ImJ1aWxkZXIiOnsiaWQiOiJodHRwczovL2dpdGh1Yi5jb20vTXlPcmcuYnVuZGxlciIsIm5hbWUiOiJNeS1CIiwiZGV0YWlscyI6eyJkaXN0cm9jb3ZlcnkiOiJqb3N0IGJ1bmRsZSJ9fSwiaW52b2NhdGlvbiI6eyJyb3NlcnZlX2lkaCI6ImFic2RlZjEyMyIsInNob3J0X2hhc2giOiJ4eHh4eHgifX0sIm1ldGFkYXRhIjp7ImNvbnRlbnRfdHlwZSI6InJlcG8iLCJi dWlsZF9jb21tYW5kIjoiY29tcGlsZSIsInRpbWVzdGFtcCI6IjIwMjUtMDQtMzBUMTY6MDA6MDBaIiwicmVjb3JkX29jY3VycmVuY3kiOnsidGltZSI6IjQ1MCJ9fX0=",
  "signatures": [
    {
      "keyid": "012abcDEF456",
      "sig": "MEUCIQD3+Jh5K...5v9Z7IA=="
    }
  ]
}

```
**DSSE Envelope**

- `"payloadType"` declares the MIME-type of the inner statement—in this case, an in-toto statement.
    
- `"payload"` is the Base64-encoded JSON attestation.
    
- `"signatures"` is an array of cryptographic signatures over the envelope, each with a `keyid` pointing to the public key and the actual `sig`.

## Decoded attestation statement
```json
{
  "subject": [
    {
      "name": "builder.page",
      "sha1": "2345f68145f8be40b1e6bbf1112f15536d47833"
    }
  ],
  "predicateData": {
    "builder": {
      "id": "https://github.com/MyOrg.builder",
      "name": "My-BI",
      "details": { "distcovery": "jost bundle" }
    },
    "invocation": {
      "roserve_id": "absdef123",
      "short_hash": "xxxxxx"
    }
  },
  "metadata": {
    "content_type": "repo",
    "build_command": "compile",
    "timestamp": "2025-04-30T16:00:00Z",
    "record_occurrence": { "time": "450" }
  }
}

```