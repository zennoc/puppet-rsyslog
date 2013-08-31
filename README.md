# Puppet module: rsyslog

This is a Puppet module for rsyslog based on the second generation layout ("NextGen") of Example42 Puppet Modules.

Made by Alessandro Franceschi / Lab42

Official site: http://www.example42.com

Official git repository: http://github.com/example42/puppet-rsyslog

Released under the terms of Apache 2 License.

This module requires functions provided by the Example42 Puppi module (you need it even if you don't use and install Puppi)

For detailed info about the logic and usage patterns of Example42 modules check the DOCS directory on Example42 main modules set.

## USAGE - Basic management

* Install rsyslog with default settings

        class { 'rsyslog': }

* Install a specific version of rsyslog package

        class { 'rsyslog':
          version => '1.0.1',
        }

* Disable rsyslog service.

        class { 'rsyslog':
          disable => true
        }

* Remove rsyslog package

        class { 'rsyslog':
          absent => true
        }

* Enable auditing without without making changes on existing rsyslog configuration files

        class { 'rsyslog':
          audit_only => true
        }


## USAGE - Overrides and Customizations
* Use custom sources for main config file 

        class { 'rsyslog':
          source => [ "puppet:///modules/example42/rsyslog/rsyslog.conf-${hostname}" , "puppet:///modules/example42/rsyslog/rsyslog.conf" ], 
        }


* Use custom source directory for the whole configuration dir

        class { 'rsyslog':
          source_dir       => 'puppet:///modules/example42/rsyslog/conf/',
          source_dir_purge => false, # Set to true to purge any existing file not present in $source_dir
        }

* Use custom template for main config file. Note that template and source arguments are alternative. 

        class { 'rsyslog':
          template => 'example42/rsyslog/rsyslog.conf.erb',
        }

* Manage directly the content of the main config file. Note that template has precedence over content.

        class { 'rsyslog':
          content => inline_template(
            file( "$settings::modulepath/example42/templates/rsyslog/rsyslog.conf.erb-${hostname}",
                  "$settings::modulepath/example42/templates/rsyslog/rsyslog.conf.erb" ) ),
        }


* Automatically include a custom subclass

        class { 'rsyslog':
          my_class => 'example42::my_rsyslog',
        }


## USAGE - Example42 extensions management 
* Activate puppi (recommended, but disabled by default)

        class { 'rsyslog':
          puppi    => true,
        }

* Activate puppi and use a custom puppi_helper template (to be provided separately with a puppi::helper define ) to customize the output of puppi commands 

        class { 'rsyslog':
          puppi        => true,
          puppi_helper => 'myhelper', 
        }

* Activate automatic monitoring (recommended, but disabled by default). This option requires the usage of Example42 monitor and relevant monitor tools modules

        class { 'rsyslog':
          monitor      => true,
          monitor_tool => [ 'nagios' , 'monit' , 'munin' ],
        }

* Activate automatic firewalling. This option requires the usage of Example42 firewall and relevant firewall tools modules

        class { 'rsyslog':       
          firewall      => true,
          firewall_tool => 'iptables',
          firewall_src  => '10.42.0.0/24',
          firewall_dst  => $ipaddress_eth0,
        }


[![Build Status](https://travis-ci.org/example42/puppet-rsyslog.png?branch=master)](https://travis-ci.org/example42/puppet-rsyslog)
