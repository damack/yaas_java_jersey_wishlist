# yaas_java_jersey_wishlist
This is an example implementation of the YaaS "Wishlist" service based on Java. It uses the RAML definition generated by the SDK without modifications. It shows how to implement a basic service and how to integrate with other services on YaaS.

License
-----------

This project is licensed under the Apache Software License, v. 2 except as noted otherwise in the LICENSE file.

API Console
-----------

You can open the API Console in a separate window by using the following link: 
- [API Console](https://api.yaas.io/hybris/java-jersey-wishlist/example)


API Overview
------------

This service provides REST endpoints for interacting with several YaaS Core and Commerce services.

### Document Service
The endpoint `/wishlists` enables you to:
- Interact with wishlists in a CRUD fashion.
  - Get a list of all wishlists within a tenant.
  - Create a new wishlist.
  - Get a specific wishlist based on an ID.
  - Update a specific wishlist based on an ID.
  - Delete a specific wishlist based on an ID.

The endpoint `/wishlists/{wishlistId}/wishlistItems` enables you to:
  - get a specific wishlist and read its items
  - create a new wishlist item and add it to the specific wishlist
  
See also [WishlistService.java](src/main/java/com/sap/wishlist/service/WishlistService.java).

### Email Service
An email is sent to the wishlist owner when a wishlist is created. For more details, have a look at the `sendMail` method in [WishlistService.java](src/main/java/com/sap/wishlist/service/WishlistService.java).

### Media Service
The endpoint `/wishlists/{wishlistId}/media` enables you to:
  - Get a list of all media for the wishlist.
  - Create a new medium.
  - Delete a specific medium based on an ID.

See also [WishlistMediaService.java](src/main/java/com/sap/wishlist/service/WishlistMediaService.java)

### Customer Service
When a wishlist is being created, the implementation checks if its owner exists as a customer. 
You can find the details at the beginning of the `post` method in [WishlistService.java](src/main/java/com/sap/wishlist/service/WishlistService.java).


Purpose & Benefits
------------------

Showcase how a service can be written using Java. Demonstrate the integration with other services on YaaS, including authentication. Topics covered:
- Usage of Spring framework
- Property handling
- Authentication with the YaaS platform
- Consumption of YaaS services
- Deployment to Cloud Foundry
- Testing


Dependencies
------------

- Core Services
  - [OAuth2 Service](https://devportal.yaas.io/services/oauth2/latest/index.html)
  - [Document](https://devportal.yaas.io/services/document/latest/index.html)
  - [Email](https://devportal.yaas.io/services/email/latest/index.html)
  - [Media](https://devportal.yaas.io/services/media/latest/index.html)
  - [Customer](https://devportal.yaas.io/services/customer/latest/index.html)


How to Build and Test
---------------------

In order to build the service locally, you need to [create an Application](https://devportal.yaas.io/gettingstarted/createanapplication/index.html) within your Project/Site, which subscribes to the following packages and scopes:
- Email package (Scopes: `hybris.email_send`, `hybris.email_manage`)
- Persistence package (`hybris.document_manage`, `hybris.document_view`)
- Media package (Scopes: `hybris.media_manage`)
- Customer Accounts package (Scope: `hybris.customer_read`)

You must then set the following environment variables:
- `YAAS_CLIENT`: Your *Application*'s *Identifier*
- `YAAS_CLIENT_ID`: Your *Application*'s *Client ID*
- `YAAS_CLIENT_SECRET`: Your *Application*'s *Client Secret*
- `YAAS_CLIENT_IS_APPLICATION`: Set this parameter in [default.properties](src/main/resources/default.properties) to `true`. (Because you are using the Application's credentials, you are running in single tenant mode. Therefore, you must set this flag to `true`.)

You must store the ID of your *Project/Site* as `TENANT` in [TestConstants.java](src/test/java/com/sap/wishlist/api/TestConstants.java).

You also need to [create a customer](https://devportal.yaas.io/services/customer/latest/index.html#CreateNewAccount) in your tenant and store it as `CUSTOMER` in [TestConstants.java](src/test/java/com/sap/wishlist/api/TestConstants.java).

Finally, use `mvn clean install` to build the service and run the tests.

To run it locally, you can call `mvn jetty:run` and navigate to the local [API Console](http://localhost:8080).

FAQ / Troubleshooting
---------------------

If you get failed tests while building with `mvn clean install`, such as `response code expected:201 but was:500"`, 
then it probably means that the test cannot connect to the Core services due to your proxy settings.

Hint: In that case, you might want to try it out with:

    mvn clean install -Dhttp.proxyPort=<proxyPort> -Dhttp.proxyHost=<proxyHost> -Dhttps.proxyPort=<proxyPort> -Dhttps.proxyHost=<proxyHost>
