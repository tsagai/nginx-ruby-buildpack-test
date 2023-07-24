# nginx-ruby-buildpack
Example of using Ruby Buildpack + Nginx Buildpack to template an Nginx.conf file

## Overview
The static buildpack starting from v1.6.0 no longer supports ERB templating, so nginx.conf files are no longer able to be templated with ERB. The Nginx buildpack from the start did not include support for ERB or any other kind of templating from the very beginning. 

This solution here can be used if there is a need to still use ERB templating of the nginx.conf file whilst using the Nginx buildpack. 
