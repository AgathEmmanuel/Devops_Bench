# OPA   

Policy-based control for cloud native environments  




Kubernetes admission control 
Microservices API authorization

application authorization

container orchestration
controlling SSH access 
providing context-based service mesh authorization


service-level authorization
end user authorization
policy-suite authorization




---
if you ask 10 teams to implement authorization for their service, you’ll end up with 10 different hardcoded implementations in different programming languages, with different enforcement guarantees and different ways of logging unauthorized access attempts.

A growing number of organizations (e.g. Netflix, Pinterest, Yelp, Chef, Atlassian) are implementing authorization by providing a unified authorization framework for all development teams. That framework provides a single language or GUI for writing policy, a single way of distributing those policies to the services that need them, a single architecture for enforcing those policies, and a single system for logging decisions.

Security and compliance teams can review the policies separately from their implementation; policies enforced in multiple places only need to be written once; policies are enforced consistently across services; decisions are logged consistently, enabling easier SIEM analysis; and policies can be updated dynamically to respond to security events.



OPA for service-level authorization

From an operational perspective, security engineers at Netflix needed to ensure that developers could tightly control which applications and services, from the vast back-end Netflix ecosystem, could call the REST API endpoint on their own service. Without such rules, Netflix services might not scale correctly to maintain high availability, as traffic pathways could become heavily congested.



OPA for end-user authorization


These policies make sure that users (living, breathing human beings) have the right levels of access in the application and can take only certain actions depending on the user’s permissions and dynamic context.
For example, in a banking application, the policy might stop someone from withdrawing $100 from an account they don’t own. In this case, OPA is a building block for developers who must either build this functionality themselves or use a policy that already exists.

OPA for product-suite authorization

A third popular way to use OPA is to unify authorization across a suite of different products, sometimes known as entitlements services in sectors like banking. Enterprises often have numerous products deployed across the organization, and they need a unified way of implementing authorization across this product suite




OPA for microservices

OPA has become very popular for helping organizations control their microservices and service mesh architectures. With OPA, you can create and enforce authorization policies directly for a microservice (typically as a sidecar), build service-to-service policies within the service mesh, or, from a security standpoint, create policies that limit lateral movement within the service mesh architecture.


OPA for Kubernetes admission control

OPA can be used to create policies that reject any ingresses that use the same host name, or that require all container images to come from a trusted registry, or to ensure that all storage is marked always with the encrypt bit, or that every app exposed to the internet use an approved domain name — to cite just a few examples.
Using OPA, administrators can set clear policies so that developers can accelerate pipeline production and rapidly bring new services to market, without worrying about operational, security, or compliance risk.
Because OPA integrates directly with the Kubernetes API server, it can reject any resource that policy disallows, across compute, networking, storage, and so on. Particularly beneficial to developers, you can expose these policies earlier in the development cycle, such as in the CI/CD pipeline, so developers can get feedback early and remediate issues before runtime. Moreover, you can even validate your policies out-of-band, ensuring that they achieve their intended effect and don’t inadvertently cause trouble.

---



## Your First OPA Policy  

"Customers should be able to view their own payments. Financial department staff should be able to view any customer payment"

```
package play

# Customers should be able to view their own payments
allow = true {
	input.method = "GET"
      input.path = ["payments", customer_id]
      input.user = customer_id
}
```




# review this snippet line by line:

    Any lines that start with the hash sign (#) are comments. It’s always a good practice to write what your policy is supposed to do as a coherent, human-readable comment.

    allow = true means that the decision would be allowed if the following ‘evaluations’ are true.

    The input method is GET. Any other HTTP method (POST, PUT, etc.) will violate the policy.

    The path is /payments/customer_id. Notice that the customer_id is not quoted, which means that it’s a variable that needs to be substituted at invocation time.

    The user should also be the customer_id.

If we were to translate this code back to plain English, it’d look something like:

Allow the request if the method it uses is GET, the path is /payments/customer_id, and the user is the same customer_id. Which effectively allows a customer to view her own payment data.  





[https://curity.io/resources/learn/curity-opa-kong-api/](https://curity.io/resources/learn/curity-opa-kong-api/)  
```

The Curity Identity Server, Open Policy Agent, and Kong Gateway all play their part in a robust architecture to provide fine-grained authorization for APIs. An identity server that leverages standards like OpenID Connect and OAuth2 is well suited to provide solid authentication and issue tokens with scopes and claims. An API Gateway like Kong can then consume this for coarse-grained authorization and further exchange of tokens to provide richer data that can hold PII information leveraged later in the chain. The API Gateway can pass that data on to an external authorization such as OPA that holds a fine-grained authorization policy capable of making an extremely detailed authorization decision. It makes a decision based on claims or attributes and shares the result with the API gateway, allowing or denying the upstream API to be called.

These components are great at what they do and can be configured to work together to provide strong security for APIs.

```



# Links  

[https://www.openpolicyagent.org/](https://www.openpolicyagent.org/)  
[https://www.infracloud.io/blogs/opa-microservices-authorization-simplified/](https://www.infracloud.io/blogs/opa-microservices-authorization-simplified/)  
[https://medium.com/microservices-learning/micoservices-security-pattern-implementing-a-policy-based-security-for-microservices-with-opa-f15164a79b0e](https://medium.com/microservices-learning/micoservices-security-pattern-implementing-a-policy-based-security-for-microservices-with-opa-f15164a79b0e)  

[https://www.infoworld.com/article/3586149/using-opa-for-cloud-native-app-authorization.html](https://www.infoworld.com/article/3586149/using-opa-for-cloud-native-app-authorization.html)  
[https://www.openpolicyagent.org/](https://www.openpolicyagent.org/)  
[https://medium.com/digio-australia/organisation-wide-user-access-management-with-open-policy-agent-opa-5815483251ba](https://medium.com/digio-australia/organisation-wide-user-access-management-with-open-policy-agent-opa-5815483251ba)  
[https://www.magalix.com/blog/introducing-policy-as-code-the-open-policy-agent-opa](https://www.magalix.com/blog/introducing-policy-as-code-the-open-policy-agent-opa)  
[Rego Playground  https://play.openpolicyagent.org/](https://play.openpolicyagent.org/)  
[https://www.openpolicyagent.org/docs/v0.11.0/guides-identity/](https://www.openpolicyagent.org/docs/v0.11.0/guides-identity/)  
[https://oxfordcomputergroup.com/resources/o365-security-native-cloud-authentication/](https://oxfordcomputergroup.com/resources/o365-security-native-cloud-authentication/)  
[https://www.infracloud.io/blogs/opa-microservices-authorization-simplified/](https://www.infracloud.io/blogs/opa-microservices-authorization-simplified/)  
[https://www.openpolicyagent.org/docs/latest/envoy-tutorial-istio/](https://www.openpolicyagent.org/docs/latest/envoy-tutorial-istio/)  
[Customizing OPA for a "Perfect Fit" Authorization Sidecar - Patrick East, Styra](https://youtu.be/uCra4Uq9bCM)  
[https://www.openpolicyagent.org/docs/latest/extensions/](https://www.openpolicyagent.org/docs/latest/extensions/)  
[https://www.openpolicyagent.org/docs/latest/rest-api/](https://www.openpolicyagent.org/docs/latest/rest-api/)  
[https://dzone.com/articles/open-policy-agent-part-iii-integrating-with-your-a](https://dzone.com/articles/open-policy-agent-part-iii-integrating-with-your-a)  
[Episode 8: Integrating Open Policy Agent with Envoy Proxy and Istio](https://youtu.be/X1sdpMkHW9U)  
[How Netflix Is Solving Authorization Across Their Cloud I - Manish Mehta & Torin Sandall, Netflix](https://youtu.be/R6tUNpRpdnY)  
[https://helpfulbadger.github.io/blog/envoy_opa_5_opa_jws/](https://helpfulbadger.github.io/blog/envoy_opa_5_opa_jws/)  
[https://www.openpolicyagent.org/docs/v0.11.0/http-api-authorization/](https://www.openpolicyagent.org/docs/v0.11.0/http-api-authorization/)  
[https://medium.com/digio-australia/organisation-wide-user-access-management-with-open-policy-agent-opa-5815483251ba](https://medium.com/digio-australia/organisation-wide-user-access-management-with-open-policy-agent-opa-5815483251ba)  
[https://medium.com/trendyol-tech/first-look-to-opa-open-policy-agent-3542810941c9](https://medium.com/trendyol-tech/first-look-to-opa-open-policy-agent-3542810941c9)  
[https://www.openpolicyagent.org/docs/latest/integration/](https://www.openpolicyagent.org/docs/latest/integration/)  
[https://curity.io/resources/learn/curity-opa-kong-api/](https://curity.io/resources/learn/curity-opa-kong-api/)  
[https://oauth.tools/](https://oauth.tools/)  
[Using Curity, OPA and Kong for end-to-end API authentication and authorization](https://youtu.be/D_STqxrJu4k)  
[https://www.infracloud.io/blogs/opa-and-gatekeeper/](https://www.infracloud.io/blogs/opa-and-gatekeeper/)  
[https://devopslearners.com/comprehensive-guide-on-integrating-open-policy-agent-opa-with-kubernetes-25efc0cb2436](https://devopslearners.com/comprehensive-guide-on-integrating-open-policy-agent-opa-with-kubernetes-25efc0cb2436)  


[https://www.magalix.com/blog/integrating-open-policy-agent-opa-with-kubernetes-a-deep-dive-tutorial](https://www.magalix.com/blog/integrating-open-policy-agent-opa-with-kubernetes-a-deep-dive-tutorial)  
[https://www.redhat.com/en/blog/open-policy-agent-part-i-%E2%80%94-introduction](https://www.redhat.com/en/blog/open-policy-agent-part-i-%E2%80%94-introduction)  
[https://www.redhat.com/en/blog/open-policy-agent-part-ii-%E2%80%94-developing-policies](https://www.redhat.com/en/blog/open-policy-agent-part-ii-%E2%80%94-developing-policies)  
[https://www.redhat.com/en/blog/open-policy-agent-part-iii-%E2%80%94-integrating-your-application](https://www.redhat.com/en/blog/open-policy-agent-part-iii-%E2%80%94-integrating-your-application)  



[https://www.openpolicyagent.org/docs/v0.12.2/kubernetes-admission-control/](https://www.openpolicyagent.org/docs/v0.12.2/kubernetes-admission-control/)  
[https://www.openpolicyagent.org/docs/v0.12.2/http-api-authorization/](https://www.openpolicyagent.org/docs/v0.12.2/http-api-authorization/)  
[https://www.infracloud.io/blogs/opa-microservices-authorization-simplified/](https://www.infracloud.io/blogs/opa-microservices-authorization-simplified/)  
[https://www.openpolicyagent.org/docs/latest/envoy-tutorial-istio/](https://www.openpolicyagent.org/docs/latest/envoy-tutorial-istio/)  


