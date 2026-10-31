# rclone: The Swiss Army Knife of Cloud Storage
- **Private keys and tokens** (S3 secret keys, service account JSON, OAuth tokens) must never be committed to version control or logged. The config file `~/.config/rclone/rclone.conf` contains sensitive data; protect it with `chmod 600`.
- **`rclone purge` ignores all filters**; it deletes everything under the specified path. Use with extreme caution.
- **`rclone sync` makes dest identical to source**; files in dest that are not in source will be DELETED. Always verify with `--dry-run` first.
# Sync (makes destination identical to source: DELETES extra files at dest)
# Use crypt remote: files are encrypted/decrypted transparently
- `s3.md`; Amazon S3 / compatible ([Official](https://rclone.org/s3/))
- `drive.md`; Google Drive ([Official](https://rclone.org/drive/))
- `dropbox.md`; Dropbox ([Official](https://rclone.org/dropbox/))
- `onedrive.md`; Microsoft OneDrive ([Official](https://rclone.org/onedrive/))
- `azureblob.md`; Azure Blob Storage ([Official](https://rclone.org/azureblob/))
- `b2.md`; Backblaze B2 ([Official](https://rclone.org/b2/))
- `googlecloudstorage.md`; Google Cloud Storage ([Official](https://rclone.org/googlecloudstorage/))
- `sftp.md`; SFTP ([Official](https://rclone.org/sftp/))
- `webdav.md`; WebDAV ([Official](https://rclone.org/webdav/))
- `swift.md`; OpenStack Swift ([Official](https://rclone.org/swift/))
- `ftp.md`; FTP ([Official](https://rclone.org/ftp/))
- And 60+ more providers; each has a page at `https://rclone.org/<name>/`
- `rclone_copy.md`, `rclone_sync.md`, `rclone_move.md`; transfer commands
- `rclone_mount.md`; FUSE mount
- `rclone_serve_*.md`; various serve modes
- `rclone_config*.md`; configuration management
- `rclone_bisync.md`; bidirectional sync
- And 80+ more commands; each has a page at `https://rclone.org/commands/<command>/`