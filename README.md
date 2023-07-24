# nginx-ruby-buildpack
Example of using Ruby Buildpack + Nginx Buildpack to template an Nginx.conf file via ERB

## Overview
The static buildpack starting from v1.6.0 no longer supports ERB templating, so nginx.conf files are no longer able to be templated with ERB in any versions beyond v1.6.0. The Nginx buildpack from the start did not include support for ERB or any other kind of templating from the very beginning. 

This solution here can be used if there is a need to still use ERB templating of the nginx.conf file whilst using the Nginx buildpack. 

# Components 
## Nginx.conf.erb
The Nginx.conf.erb functionally replaces Nginx.conf. We have all of our templating included in this file. The Ruby buildpack, when placed first in the order of buildpacks in the `Nginx_manifest.yml` file will run the ERB templating on this file. 

## Nginx.conf
This is a dummy Nginx.conf file. It is just used to trick the Nginx buildpack to avoid hitting an error whilst using the Nginx buildpack. 

## Nginx_manifest.yml
The manifest YML file for our test application. Note that the environment variables are defined which are `APP_ROOT` and `FORCE_HTTPS`. These variables have to be included in the ERB templating via the ERB syntax `<%= ENV["APP_ROOT"] %>` and `<%= ENV["FORCE_HTTPS"] %>`. Including them via Nginx variable format via `{{APP_ROOT}}` and `{{FORCE_HTTPS}}` will result in a failure. 

Also, a very important point is that the Ruby buildpack must be placed first before the Nginx buildpack to ensure that the ERB templating is done before the Nginx Buildpack is launched. 

## Procfile
This file runs the ERB templating that occurs in the Ruby buildpack. In the Procfile command `web: erb nginx.conf.erb | tee nginx.conf && varify -buildpack-yml-path ./buildpack.yml ./nginx.conf $HOME/modules $DEP_DIR/nginx/modules && nginx -p $PWD -c ./nginx.conf` , We see that the `nginx.conf.erb` file is piped into the `nginx.conf` file and the Nginx buildpack verification is called. The newly piped `nginx.conf` file is provided as input to the Nginx `varify` process. 
