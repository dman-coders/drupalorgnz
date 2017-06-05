
## Get a local copy of live

* Set up a drush site-alias to communicate with LIVE.
* Set up a fake drush site-alias to stand in as a live mirror.
  * by making a site docroot for d6 files to be copied into.
    `mkdir d6`
  * and making a D6 DB to downsync into.
    `sudo su; mysql`
    `CREATE DATABASE drupal_d6';`
    `GRANT ALL PRIVILEGES ON drupal_d6.* TO 'drupal_d6'@'%' IDENTIFIED BY 'drupal_d6';`
  * But not bothering to turn the site on with a vhost.
  * Add the D6 mirror DB credentials to the site-alias directly.
    And we should be able to refer to a copy od the D6 DB for the purposes of migrations.
  * (Sorta failed, add the db credentials to a fake settings.php also, as drush was getting confused with a few shadow site folders).
  
#### Down-sync from LIVE to D6 Mirror

    drush sql-sync -vd @d6.drupal.org.nz @d6.drupal.org.nz.local

## Install pre-requisites on D8

## Use Drupal migrate UI

Enter the DB credentials to let D8 see D6.

## Review Upgrade

See how many missing upgrade paths we need to worry about.
