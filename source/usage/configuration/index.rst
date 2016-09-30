.. _configuration:

#############
Configuration
#############

Paramétrage du php.ini en mode développement
============================================

affichage des notices et pas des erreurs deprecated  dans etc/php5/apache2/php.ini (sous linux) ::

  error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT
  display_errors = On

