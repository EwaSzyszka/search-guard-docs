---
title: SAML authentication
html_title: Kibana SAML
slug: kibana-authentication-saml
category: kibana-authentication
order: 800
layout: docs
edition: enterprise
description: How to configure Kibana for SAML Single Sign On authentication and IdP integrations.
resources:
  - "https://search-guard.com/kibana-elasticsearch-saml/|Using SAML for Kibana Single Sign-On (blogpost)"


---
<!---
Copyright 2019 floragunn GmbH
-->

# Using Kibana with SAML authentication
{: .no_toc}

{% include toc.md %}

Since most of the SAML specific configuration is done in Search Guard, just activate SAML in your `kibana.yml` by adding:

```
searchguard.auth.type: "saml"
```

In addition the Kibana endpoint for validating the SAML assertions must be whitelisted:

```
server.xsrf.whitelist: ["/searchguard/saml/acs"]
```

If you use the logout POST binding, you also need to whitelist the logout endpoint:

```
server.xsrf.whitelist: ["/searchguard/saml/acs", "/searchguard/saml/logout"]
```

## IdP initated SSO

To use IdP initiated SSO, in your IdP, set the *Assertion Consumer Service* endpoint to:

```
/searchguard/saml/acs/idpinitiated
```

Then add this endpoint to the xsrf whitelist in kibana.yml:

```
server.xsrf.whitelist: ["/searchguard/saml/acs/idpinitiated", "/searchguard/saml/acs", "/searchguard/saml/logout"]
```