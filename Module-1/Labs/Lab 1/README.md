# Lab 1 - Design & Create an API Proxy with OpenAPI Specification

*Duration : 15 mins*

*Persona : API Team*

# Use case

You have a requirement to create a reverse proxy to handle requests from the Internet and forward them to an existing service. You have decided to follow a design first approach and built a reusable component, a specification which can be used to describe the API contract, generate API documentation, generate API test cases, etc., using the OpenAPI Specification. You would like to generate an Apigee API Proxy by using the OpenAPI Specification (fka: Swagger) instead of building the API Proxy from scratch.

# How can Apigee API Management help?

Apigee enables you to quickly expose services as APIs.  You do this by creating an [**API proxy**](https://docs.apigee.com/api-platform/fundamentals/understanding-apis-and-api-proxies#whatisanapiproxy), which provides a facade for the service that you want to expose, such as existing API endpoints, generic HTTP services, or applications (such as Node.js). The API proxy decouples your service implementation from the API endpoint that developers consume. This shields developers from future changes to your services as well as implementation complexities. As you update services, developers, insulated from those changes, can continue to call the API, uninterrupted.
On Apigee, the API Proxy is also where runtime policy configuration is applied for API Management capabilites. For further information, please see: [Understanding APIs and API Proxies](https://docs.apigee.com/api-platform/fundamentals/understanding-apis-and-api-proxies#whatisanapiproxy).

![Image of Apigee Proxy flow execution order](./media/ProxyToBackendWithFlows_v3.png)

Apigee also supports the [**OpenAPI specification**](https://github.com/OAI/OpenAPI-Specification) out of the box, allowing you to auto-generate API Proxies. 

In this lab, we will 
* View an OpenAPI specification for an existing HTTP service, and
* create an API proxy that routes inbound requests to an existing HTTP service.

# Pre-requisites

* Basic understanding of [OpenAPI Specification](https://github.com/OAI/OpenAPI-Specification) (FKA: Swagger)
* Access to an HTTP client to test the API (eg. cURL, Postman, Hoppscotch, etc.). If you do not have access to one, you can use the [Apigee Trace Tool](https://docs.apigee.com/api-platform/debug/using-trace-tool-0).

# Instructions

**Note: During this workshop, as you may be working within an [Apigee Organization (Org)](https://docs.apigee.com/api-platform/fundamentals/apigee-edge-organization-structure) that is shared by multiple users.  Please prefix all asset names within the Org with your initials. For example, Spec name = {your-initials}\_{spec name}, API proxy name = {your-initials}\_{proxy name}, etc.**

## View an OpenAPI Specification

During the course of this lab, the sample HTTP service we will expose as an API endpoint is the Hipster Products service located at [http://cloud.hipster.s.apigee.com/products](http://cloud.hipster.s.apigee.com/products).
First, we are going to view the OpenAPI specification for the different resource endpoints, i.e. /products and /products/{productId}.

1. Go to URL: [https://raw.githubusercontent.com/apigee/apijam/master/Module-1/Resources/products-catalog-spec.yaml](https://raw.githubusercontent.com/apigee/apijam/master/Module-1/Resources/products-catalog-spec.yaml)
2. Here you can see the raw yaml for the service which we'll be generating in the next step.
  * version 2.0
  * host
  * basePath
  * Paths and Verbs
  * Responses and Definitions

## Create an API Proxy

1. It’s time to create Apigee API Proxy from an OpenAPI Specification. Click on **Develop → API Proxies** from side navigation menu.

![Image describing how to navigate to the API Proxies view.  Home - Develop - API Proxies](./media/image_5.png)

2. Click **+Proxy** The Build a Proxy wizard is invoked.

![Image showing the location of the button to create an API Proxy.](./media/image_6.png)

3. Select **Reverse proxy**, Click on **Use OpenAPI** below reverse proxy option.

![Image showing selection of API Proxy type.  User has selected Reverse Proxy and clicked "Use OpenAPI Spec"](./media/image_7.png)

4. Use the url from above here to have apigee ipmort your spec

5. Enter details in the proxy wizard. Replace **{your-initials}** with the initials of your name.

    * Proxy Name: **{your_initials}**\_Hipster-Products-API

    * Proxy Base Path: /v1/**{your_initials}**\_hipster-products-api

    * Existing API: Observe the field value which is auto filled from OpenAPI Spec.

![Image showing filling out of Proxy details.  User has added his or her initials as the prefix for "Name" and "Base path" per lab instructions.](./media/image_10.png)

6. Verify the values and click **Next**.

7. Select **Pass through (none)** for the authorization in order to choose not to apply any security policy for the proxy. Click Next.

![Image describing Policy selection in API Proxy Wizard.  User has selected "Pass through".](./media/image_12.png)

8. You can select which API resources, from the list configured in the OpenAPI Spec, should be exposed. Select all & Click on **Next**

![Image describing ability to select or deselect individual operations impoted from OpenAPI Spec.  User has selected the default, which is all operations.](./media/image_11.png)

9. Go with the **secure Virtual Host** configuration. Ensure that the **default** one is unchecked.

![Image describing how to enable or disable the http and/or https virtual hosts.  User has unchecked "default" so only secure (https) is checked.](./media/image_13.png)

10. Ensure that only the **test** environment is selected to deploy to and click **Create and Deploy**

![Image describing selection of which environments to deploy API Proxy to via the wizard.  User has sselected "test" and left other environments unchecked.](./media/image_14.png)

11. Once the API proxy is created and deployed click **Edit Proxy** to view your proxy in the proxy editor.

![Image describing completion of wizard.  There is an "Edit proxy" button that will open the newly created proxy in the proxy editor.](./media/image_15.png)

12. *Congratulations!* ...You have now built a reverse proxy for an existing backend service. You should see the proxy **Overview** tab.

![Image describing various features of the API Proxy overview page.  Details about proxy revision, deployment state, virtual hosts, and proxy and target endpoints can be seen here.  Additionally there are links to open the "develop" view or the "trace" tool.](./media/image_16.png)

## Test the API Proxy
Let us test the newly built API proxy. You can use any HTTP client like cURL or Postman, or the [Apigee Trace Tool](https://docs.apigee.com/api-platform/debug/using-trace-tool-0).

### Using cURL

org = Organization name
env = Environment where API is deployed

```
curl -X GET "https://{{org}}-{{env}}.apigee.net/{{your initials}}_hipster-products-api/products"
```

### Using Trace Tool:

* Navigate to your proxy's **Trace** tab.

* Ensure that the deployed API revision is selected.

* Hit **Start Trace Session**.

![Image describing the API Proxy Trace Tool.  User has selected revision 1 to trace and then clicked the button labeled "Start Trace Session"](./media/trace-tool-start.png)

* Wait for Trace session to start.

* Modify the URL to send to a valid API resource - **append a '/products' to the end of the URL.**

* Hit 'Send'.

![Image describing invoking the trace tool.  User appended "/products" to the URL and clicked  the "send" button.](./media/trace-tool-send.png)

* You will see that the API proxy recieved the request and sent back a HTTP status 200 response which was logged by the Trace session. You can click on the step shown below to view the response body.

![Image depicting a successful trace response.  Left pane shows the HTTP method, status code, invoked URI, and elapsed response time.  User has also clicked a node in the visual transaction map that reveals the JSON response received from the server.](./media/trace-tool-response.png)


## Save the API Proxy

1. Let’s save the API Proxy locally as an API Bundle so that we can reuse it in other labs.

2. Save the API Proxy by downloading the proxy bundle, See screenshot below for instructions.

![Image describing the process of downloading an API Proxy locally.  User is on the "Overview" page of the API Proxy and has clicked "Project" then "Download Revision"](./media/image_20.png)

# Lab Video

If you like to learn by watching, here is a short video on creating a reverse proxy using OpenAPI Specification - [https://www.youtube.com/watch?v=3XBG9QOUPzg](https://www.youtube.com/watch?v=3XBG9QOUPzg)

# Earn Extra-points

Now that you have created a reverse proxy using an OpenAPI spec, click on the Develop tab and explore the flow conditions populated from the OpenAPI spec.  Further expore the trace tab in the API Proxy editor which supports filtering traces as well as downloading trace data for offline use.  Also, explore the OpenAPI Spec editor which allows you to edit an OpenAPI specification and supports bidirectional navigation between the yaml/json and a live view of the rendered OpenAPI spec.  See how you can generate an API Proxy using the "Generate proxy" action in the Specs view.
![Picture of "Generate proxy" action in Specs view](./media/image_21.png)

# Quiz

1. How do you import the proxy bundle you just downloaded?
2. How does Apigee handle API versioning?
3. Are there administrative APIs to create, update, or delete API proxies in Apigee?

# Summary

That completes this hands-on lesson. In this simple lab you learned how to use Apigee to proxy an existing backend using an OpenAPI Specification and the Apigee proxy wizard.

# References

* Useful Apigee documentation links on API Proxies -

    * Build a simple API Proxy - [http://docs.apigee.com/api-services/content/build-simple-api-proxy](http://docs.apigee.com/api-services/content/build-simple-api-proxy)

    * Best practices for API proxy design and development - [http://docs.apigee.com/api-services/content/best-practices-api-proxy-design-and-development](http://docs.apigee.com/api-services/content/best-practices-api-proxy-design-and-development)

* Watch this "4 Minute Video 4 Developers" (4MV4D) on "Anatomy of an API Proxy" - [https://youtu.be/O5DJuCXXIRg](https://youtu.be/O5DJuCXXIRg)

# Rate this lab

How did you like this lab? Rate [here](https://goo.gl/forms/G8LAPkDWVNncR9iw2).

Now go to [Lab-2](../Lab%202)
