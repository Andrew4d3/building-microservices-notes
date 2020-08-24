# Chapter 9: Security

---

## How do I identify what it's being authenticated? (abstractly speaking)

Generally, when we’re talking abstractly about who or what is being authentica‐
ted, we refer to that party as the principal.

---

## What's authorization?

Authorization is the mechanism by which we map from a principal to the action we
are allowing her to do. Often, when a principal is authenticated, we will be given
information about her that will help us decide what we should let her do. We might,
for example, be told what department or office she works in—pieces of information
that our systems can use to decide what she can and cannot do.

---

## How does a SSO solution work?

When a principal tries to access a resource (like a web-based interface), she is directed to authenticate with an identity provider. This may ask her to provide a username
and password, or might use something more advanced like two-factor authentication.
Once the identity provider is satisfied that the principal has been authenticated, it
gives information to the service provider, allowing it to decide whether to grant her
access to the resource.

---

## How can we use API gateways with SSO solutions?

Rather than having each service manage handshaking with your identity provider,
you can use a gateway to act as a proxy, sitting between your services and the outside
world (as shown in Figure 9-1). The idea is that we can centralize the behavior for
redirecting the user and perform the handshake in only one place.

![image](https://user-images.githubusercontent.com/1868409/90997068-72102980-e58e-11ea-9166-9b20a3c7c28d.png)

---

## What's the downside of using API gateways with SSO's?

I have seen some people put all their eggs in one basket, relying on the
gateway to handle every step for them. And we all know what happens when we have
a single point of failure.

---

## When defining principal roles, why should we keep those roles local to the microservice in question?

These decisions need to be local to the microservice in question. I have seen people
use the various attributes supplied by identity providers in horrible ways, using really
fine-grained roles like CALL_CENTER_50_DOLLAR_REFUND, where they end up
putting information specific to one part of one of our system’s behavior into their
directory services. This is a nightmare to maintain and gives very little scope for our
services to have their own independent lifecycle, as suddenly a chunk of information
about how a service behaves lives elsewhere, perhaps in a system managed by a different part of the organization.

---

## Why should HTTP basic authentication be used over HTTPS?

When using HTTPS, the client gains strong guarantees that the server it is talking to
is who the client thinks it is. It also gives us additional protection against people
eavesdropping on the traffic between the client and server or messing with the payload.

---

## What's the downside of using HTTPS? And how can you work around it?

Another downside is that traffic sent via SSL cannot be cached by reverse proxies like
Varnish or Squid. This means that if you need to cache traffic, it will have to be done
either inside the server or inside the client. You can fix this by having a load balancer
terminate the SSL traffic, and having the cache sit behind the load balancer.

---

## Explain TLS

Another approach to confirm the identity of a client is to make use of capabilities in
Transport Layer Security (TLS), the successor to SSL, in the form of client certificates.
Here, each client has an X.509 certificate installed that is used to establish a link
between client and server. The server can verify the authenticity of the client certificate, providing strong guarantees that the client is valid.

---

## Using TLS has its complications. So when should we use it?

Using wildcard certificates can help,
but won’t solve all problems. This additional burden means you’ll be looking to use
this technique when you are especially concerned about the sensitivity of the data
being sent, or if you are sending data via networks you don’t fully control. So you
might decide to secure communication of very important data between parties that is
sent over the Internet, for example.

---

## What important consideration should we bear in mind when using strategies like JWT?

Finally, understand that this approach ensures only that no third party has manipulated the request and that the private key itself remains private. The rest of the data in
the request will still be visible to parties snooping on the network.

---

## What two aproches do we have when using API keys, and how do we manage them?

Some systems use a single API key that is shared, and use an approach similar to
HMAC as just described. A more common approach is to use a public and private
key pair. Typically, you’ll manage keys centrally, just as we would manage identities of
people centrally. The gateway model is very popular in this space.

---

## Explain the type of vulnerability called "confused deputy problem"

There is a type of vulnerability called the confused deputy problem, which in the context of service-to-service communication refers to a situation where a malicious party
can trick a deputy service into making calls to a downstream service on his behalf
that he shouldn’t be able to. For example, as a customer, when I log in to the online
shopping system, I can see my account details. What if I could trick the online shop‐
ping UI into making a request for someone else’s details, maybe by making a call with
my logged-in credentials?
