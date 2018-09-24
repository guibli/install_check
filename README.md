#!/bin/bash -e
cd /var/www/drupal_repo/
#check existence of drupal install

if drush status bootstrap | grep -q Successful ; then
    echo OK
else
    echo FAIL
    #site install standard
    drush -y -vvv site-install minimal --db-url=mysql://user:password@localhost:3306/
    #set uui to marqueblanche
    drush -y config-set "system.site" uuid "##################################"
    #delete shortcut blocking cim
    drush ev '\Drupal::entityManager()->getStorage("shortcut_set")->load("default")->delete();'
fi
drush cim -y -v
drush cr
