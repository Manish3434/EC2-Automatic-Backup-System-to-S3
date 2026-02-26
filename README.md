# EC2-Automatic-Backup-System-to-S3
Implemented an EC2 Automatic Backup System to Amazon S3 using IAM Roles for secure access without hardcoded credentials. Developed a Bash script to automate file backups and scheduled it using cron to run daily. Ensured secure, reliable, and cost-effective cloud storage with role-based authentication and AWS CLI integration.
sudo vi /usr/local/bin/backup_to_s3.sh
#inside the vim file 
#!/bin/bash
BUCKET_NAME="myec2backup12"
SOURCE_DIR="/home/ec2-user/important_data"
TEMP_DIR="/tmp/backup_temp"
DATE=$(date +%Y-%m-%d)
TIME=$(date +%H-%M-%S)
BACKUP_FILE="backup-$DATE-$TIME.tar.gz"
echo "Starting Backup: $DATE $TIME"
mkdir -p $TEMP_DIR
echo "Compressing files..."
tar -czf $TEMP_DIR/$BACKUP_FILE $SOURCE_DIR
echo "Uploading to S3..."
aws s3 cp $TEMP_DIR/$BACKUP_FILE s3://$BUCKET_NAME/
if [ $? -eq 0 ]; then
	echo "Upload Successful!"
	rm $TEMP_DIR/$BACKUP_FILE
else
	echo "Upload Failed! Check logs."
fi


sudo chmod +x /usr/local/bin/backup_to_s3.sh
/usr/local/bin/backup_to_s3.sh
Sudo yum install cronie -y
sudo systemctl enable crond
 sudo systemctl start crond
sudo systemctl status crond
Crontab  -e
# inside the crone file Put the given syntax for timing backup
0 2 * * * /usr/local/bin/ec2-s3-backup.sh >> /var/log/ec2-backup.log 2>&1

