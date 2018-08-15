# Keycloak Demo

This demo shows how to run Keycloak and secure different applications and services.

There are a few bits to the demo and to make it easy to deploy everything it uses MiniShift to make it 
easy to run everything on a single computer and have everything wired together properly.

Before starting the demo you need to download and install MiniShift. This is as easy as [downloading the
MiniShift distribution](https://github.com/minishift/minishift#getting-started).

Once you have downloaded MiniShift start it up and make sure you install the admin-user addon. For 
simplicity you can just run `bin/start-minishift.sh` which will do this for you.

## Overview

The demo contains the following parts:

* Keycloak
* Node.js REST service
* PHP REST service
* HTML5 application

## Starting Keycloak

Simply run `bin/start-keycloak.sh` and Keycloak will be deployed to OpenShift. 

To find the hostname of Keycloak run `oc get routes keycloak`. Then open https://<hostname> in your
favorite browser.

You can now login to the Keycloak admin console with username `admin` and password `admin`.

To configure Keycloak you can either create a new realm in the admin console and import `demo-realm.json`
or run `bin/configure-keycloak.sh`.

## Deploying the Node.js service

The Node.js service is a very simple API that is secured with the Keycloak Node.js adapter. 

Simply run `bin/start-service.sh` and the service will be deployed to OpenShift. 

To find the hostname of the service run `oc get routes demo-service`. Then open `https://<hostname>/public` 
in your favorite browser. You can also try `https://<hostname>/secured`, but this will return a 401 as you
are not authenticated at this point.

## Deploying the PHP service

The PHP service is a very simple API that is secured with the Keycloak Generic adapter.

Simply run `bin/start-service-php.sh` and the service will be deployed to OpenShift. 

To find the hostname of the service run `oc get routes demo-service-php`. Then open `https://<hostname>/public` 
in your favorite browser. You can also try `https://<hostname>/secured`, but this will return a 401 as you
are not authenticated at this point.

## Deploying the HTML5 application

The HTML5 application is a basic web application that is secured with the Keycloak JavaScript adapter.
You can login to the application through Keycloak and then securely invoke the Node.js service.

Simply run `bin/start-app-php.sh` and the application will be deployed to OpenShift.
 
To find the hostname of the service run `oc get routes demo-app`. Then open `https://<hostname>/public` 
in your favorite browser. Click on the login button and login with username `keycloak` and password `test`.
Now you can click on the various buttons to invoke different endpoints on the service.

You won't be able to invoke the admin endpoint at this point as the `keycloak` user doesn't have the required
roles. Login to the Keycloak admin console and add the `admin` role to the `keycloak` user to be able to
do that.

One last fun thing you can try is to change the application to invoke the PHP service instead.