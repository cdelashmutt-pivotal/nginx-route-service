# nginx-route-service 
(Ab)using NGINX as a caching proxy route service in Cloud Foundry.

## Preparing to use the Service
### Configuration
The configuration for NGINX is in the `nginx.conf` file in this repo.

By default, the NGINX server will be configured to cache to local disk, use local memory to store keys, and retain responses for 1 minute.  This configuration likely will need to be changed for your application.  Refer to http://nginx.org/en/docs/http/ngx_http_proxy_module.html for more options on configuration.  Particularly, you will probably will be interested in the `location` block inside the `server` block to modify caching parameters.

### Pushing to Cloud Foundry
1. Clone this repo.
2. Call `cf push` in the directory you cloned to push the app with defaults.
3. Create a user provided service, specifying the full domain name for the nginx-route-service app you pushed above.  Us this command (replacing `<app-domain>` with the application domain that your nginx-route-service app is using): `cf cups cache -r https://nginx-route-service.<app-domain>`

## Binding the Service to a Route
Bind the service to any routes you want to enable caching on with the following command, replacing `<app-domain>` and `<hostname>` with the values applicable to your specific route: `cf brs <app-domain> cache --hostname <hostname>`.

For example, if you had an application bound to the hostname "foo" and the application domain "bar.com", then you could enable caching on that route by calling `cf brs bar.com cache --hostname foo`. 
