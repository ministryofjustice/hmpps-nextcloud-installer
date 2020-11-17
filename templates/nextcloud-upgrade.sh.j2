#!/usr/bin/env bash

####VARS
NEXTCLOUD_VERSION=$1
INSTALLER_URL="https://download.nextcloud.com/server/releases/nextcloud-$NEXTCLOUD_VERSION.zip"
ZIPPED_INSTALLER="nextcloud-${NEXTCLOUD_VERSION}.zip"
UPGRADE_BACKUP_DIR="/tmp/upgrade-backups"
HTML_DIR="/var/www/html"
CURRENT_VERSION=$(sudo -u {{ web_user }} php {{ nextcloud_dir }}/occ -V | grep -v "Nextcloud or" | grep Nextcloud | awk '{print $2}')
BACKUP_NEXT_CLOUD_DIR="${UPGRADE_BACKUP_DIR}/{{ app_name }}-${CURRENT_VERSION}"
service="httpd"

if [[ -z $NEXTCLOUD_VERSION ]]; then
  echo '--------------------------------------------------------------------------------------------------------'
  echo "Usage $0 NextCloud_Version"
  echo "NextCloud_Version: 20.0.0 | 19.0.4 | 18.0.10 | 17.0.10"
  echo "Performs Nexcloud upgrade on Server"
  echo '--------------------------------------------------------------------------------------------------------'
  exit 1
fi

failure ()
{
echo "--->Failure ...exiting"
exit 1
}

backup ()
{
echo "---> Creating Backup folder"
sleep 3
mkdir -p $BACKUP_NEXT_CLOUD_DIR         || failure
echo "---> Backing up Nextcloud config, app and themes"
sleep 3
cp -v --preserve {{ nextcloud_dir }}/config/config.php $BACKUP_NEXT_CLOUD_DIR  || failure
cp -rv --preserve {{ nextcloud_dir }}/apps $BACKUP_NEXT_CLOUD_DIR/             || failure
cp -rv --preserve {{ nextcloud_dir }}/themes $BACKUP_NEXT_CLOUD_DIR/           || failure
cd {{ nextcloud_dir }} || failure
echo "--->Backing up config to ${BACKUP_NEXT_CLOUD_DIR}/nextcloud-config.json"
sleep 3
sudo -u {{ web_user }} php occ config:list --private > ${BACKUP_NEXT_CLOUD_DIR}/export-nextcloud-config.json || failure
echo "--->Stopping $service service"
sleep 3
systemctl stop $service  || failure
cd $HTML_DIR
rm -rf {{ nextcloud_dir }}
}

download_version ()
{
echo "---> Downloading Nextcloud Version: $NEXTCLOUD_VERSION"
sleep 3
aws s3 cp s3://tf-{{ region }}-hmpps-delius-{{ env_type }}-{{ app_name }}-backups/${ZIPPED_INSTALLER} . || curl $INSTALLER_URL --output $ZIPPED_INSTALLER || failure
echo "--->Extracting $ZIPPED_INSTALLER"
sleep 3
unzip $ZIPPED_INSTALLER  || failure
rm -f *.zip
}

restore ()
{
echo "---> Restoring config.php from backup"
sleep 3
yes| cp -v --preserve ${BACKUP_NEXT_CLOUD_DIR}/config.php {{ nextcloud_dir }}/config/config.php || failure
chown -R {{ web_user }}:{{ web_user }} {{ nextcloud_dir }}
}

alter_table ()
{
if [[ "$NEXTCLOUD_VERSION" == "18.0.10" ]] && [[ "$CURRENT_VERSION" == "17.0.10" ]]; then
  echo "---> Adding Column: entity to Table: oc_flow_operations"
  sleep 3
  DB_HOST={{ db_dns_name }}
  DB_USER={{ nextcloud_rds_db_user }}
  DB_PASS=$(aws ssm get-parameters --with-decryption --names "{{ nextcloud_rds_db_param }}" --region "{{ region }}" --query "Parameters[0]"."Value" | sed 's:^.\(.*\).$:\1:')
  mysql -h $DB_HOST -u $DB_USER -p"$DB_PASS" {{ app_name }} -e "alter table oc_flow_operations add column entity character varying(256) not null ;" || failure
fi
}

upgrade ()
{
echo "---> Starting $service service"
sleep 3
systemctl start $service                 || failure
cd {{ nextcloud_dir }}
echo "---> Commencing upgrade to $NEXTCLOUD_VERSION"
sleep 3
sudo -u {{ web_user }} php occ upgrade        || failure
}

##MAIN
if [[ "$NEXTCLOUD_VERSION" == "$CURRENT_VERSION" ]]; then
  echo "---> Nextcloud Version is already: $CURRENT_VERSION ...exiting"
  failure
fi

backup
download_version
restore
alter_table
upgrade
