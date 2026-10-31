# pg_backup.sh: PostgreSQL logical backup
# pg_basebackup.sh: PostgreSQL physical backup for PITR
# mysql_backup.sh: MySQL logical backup
# xtrabackup.sh: MySQL physical backup
# mongo_backup.sh: MongoDB backup
# s3_upload.sh: Upload backups to S3
# Prune old snapshots: keep 7 daily, 4 weekly, 6 monthly
# verify_backup.sh: weekly restore test
  echo "ALERT: Backup verification failed; users table is empty" >&2
# backup_all.sh: unified backup orchestrator