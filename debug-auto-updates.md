# Debugging Auto-Updates

## Log File

The updater writes debug logs to `/tmp/bfloat-updater.log`. This bypasses Electron's stdout buffering in packaged apps where `console.log` output is not visible.

```bash
# Watch logs in real-time
tail -f /tmp/bfloat-updater.log

# Check the last 50 lines
tail -50 /tmp/bfloat-updater.log

# Clear the log before a fresh test
rm /tmp/bfloat-updater.log
```

## What the Log Shows

Each line is timestamped and prefixed with `[Updater]`. The log covers the full update lifecycle:

1. **Startup** — app version, platform, arch, whether it was relaunched by the updater
2. **Feed URL** — where the app checks for updates (S3 bucket)
3. **Update check** — whether a newer version was found on S3
4. **Download progress** — percentage and bytes transferred
5. **Download complete** — the version that was downloaded
6. **Install result** — success or error (e.g. code signature mismatch)

## Common Issues

### No update detected
- Check that `latest-mac.yml` exists on S3: `curl https://bfloat-workbench-updates.s3.us-west-2.amazonaws.com/latest-mac.yml`
- The version in `latest-mac.yml` must be **higher** than the installed app version
- Log will show: `No update available (current: X, remote: Y)`

### 403 Forbidden on update check
- `latest-mac.yml` is missing from S3 (deleted or never uploaded)
- Log will show: `checkForUpdates failed: 403 Forbidden`

### SHA512 checksum mismatch
- The binary on S3 doesn't match the hash in `latest-mac.yml`
- Usually caused by a partial/interrupted upload — re-upload the build artifacts
- Log will show: `sha512 checksum mismatch, expected ..., got ...`

### Code signature validation failed
- The downloaded update's signature doesn't match what the installed app expects
- Happens when a locally built (ad-hoc signed) app tries to install a CI-signed update, or vice versa
- Both the installed app and the update must be signed by the same identity
- Log will show: `Code signature at URL ... did not pass validation`

## Verifying S3 State

```bash
# Check what version S3 is serving
curl https://bfloat-workbench-updates.s3.us-west-2.amazonaws.com/latest-mac.yml

# Check if the binary actually exists (200 = exists, 403 = missing)
curl -sI https://bfloat-workbench-updates.s3.us-west-2.amazonaws.com/Bfloat-0.2.XX-arm64-mac.zip | head -1

# List all files in the bucket (requires AWS credentials)
aws s3 ls s3://bfloat-workbench-updates/
```

## Testing the Update Flow Locally

1. Build a local app with a **lower** version number than what's on S3
2. Set `forceCodeSigning: false` in `electron-builder.yml` for the local build
3. Run `pnpm build:mac:dev` from `apps/desktop/`
4. Re-sign: `codesign --deep --force --sign - dist/mac-arm64/Bfloat.app`
5. Copy to `/Applications`: `cp -R dist/mac-arm64/Bfloat.app /Applications/Bfloat.app`
6. Launch the app and watch `/tmp/bfloat-updater.log`
7. Revert `forceCodeSigning: true` and `package.json` version when done

Note: install will fail with code signature mismatch if the local build is ad-hoc signed and S3 has a CI-signed build. The detection and download steps will still work for validation.
