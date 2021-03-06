# Cachet for OpenShift
[![OpenShift](http://launch-shifter.rhcloud.com/launch/Cachet On.svg)](https://openshift.redhat.com/app/console/application_type/custom?&cartridges%5B%5D=http://cartreflect-claytondev.rhcloud.com/github/boekkooi/openshift-cartridge-nginx&cartridges%5B%5D=http://cartreflect-claytondev.rhcloud.com/github/boekkooi/openshift-cartridge-php&cartridges%5B%5D=mysql-5.5&cartridges%5B%5D=cron-1.4&initial_git_url=https://github.com/ALinuxNinja/openshift-cachet.git&name=cachet&initial_git_branch=master)

This installs the 2.x version of Cachet.

Cachet for OpenShift automatically downloads the latest version of Cachet 2.x and installs it in a PHP-FPM based setup.

## How to install
To install, just click the OpenShift button above.

## Enabling Mail
Mail is disabled by default. To enable it, fill in your credentials in $OPENSHIFT_REPO_DIR/Cachet/.env, and run the following command in $OPENSHIFT_REPO_DIR/Cachet
```
$OPENSHIFT_PHP_VERSION_DIR/bin/php artisan config:cache
```

## Upgrading
There is an experimental script called 'mgmutil' in $OPENSHIFT_DATA_DIR/bin
Note that the script is experimental and may cause issues with your installation.

Be sure to grab the latest script by running something like
```
curl -L https://raw.githubusercontent.com/ALinuxNinja/openshift-cachet/master/.openshift/data/mgmutil > $OPENSHIFT_DATA_DIR/bin/mgmutil
```

Note that if you have a big installation, the backup may not restore properly in the small gear due to disk quotas. Not much can be done about that, you will have to get a larger gear.

### PHP
To upgrade PHP, see instructions at https://github.com/boekkooi/openshift-cartridge-php
By default, this uses the latest version of PHP avaliable at the time of launching the application.

## Backup and Restore
Mgmutil, located in $OPENSHIFT_DATA_DIR/bin allows for backups to be created. Backups are automatically created during an upgrade, and can be used to restore the deployment if an upgrade fails. If a upgrade fails, that is the fastest way to get your app back, so make sure you do a backup.
The generated backup is stored in $OPENSHIFT_DATA_DIR/backup.tar.gz
Note that mgmutil will only do a backup of the MySQL tables and the files in $OPENSHIFT_REPO_DIR/Cachet. If you want to add extra files to the backup, place them in $OPENSHIFT_REPO_DIR/Cachet or modify mgmutil. I do not provide support for backups created by a modified mgmutil.

To restore, use the following process:
  1. Deploy an application using the deploy button at the top of this README
  2. After the application is deployed, use git to clone the application using the URL under "Source Code" in the OpenShift Online application.
  3. Place backup.tar.gz in the root of the cloned repo.
  4. Add backup.tar.gz to the git repo and commit it.
  5. Do a `git push origin`

The backup will then be restored.
