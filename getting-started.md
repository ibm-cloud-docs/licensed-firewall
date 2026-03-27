---

copyright:
  years: 2026
lastupdated: "2026-03-27"

keywords:

subcollection: licensed-firewall

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with the licensed firewall for FortiGate
{: #getting-started}

_The short description should be a single, concise paragraph that contains one or two sentences and no more than 50 words. Briefly mention what the user's learning goal is and include the following SEO keywords in the title short description: IBM Cloud, ServiceName, tutorial. If the release phase of your service is experimental or beta, be sure to indicate that in the first occurrence of the service name, for example, Cost and Asset Management (Experimental)._ For example: "In this getting started tutorial, we'll take you through a sample node.js ToDo app that will take you about 10 minutes to deploy."
{: shortdesc}

## Before you begin
{: #prereqs}

_There should be a one sentence intro to the prereqs. If you don't have prereqs, remove this section_ For example: "You need an [{{site.data.keyword.Bluemix}} account](https://cloud.ibm.com/registration/), an instance of the _ServiceName_ service, and the following commands to check if you are properly set up."



## _Title should be task oriented and descriptive_
{: #anchor_value}
{: step}



_If you have substeps, introduce them and then follow with the steps as paragraph chunks. Do not use numbers or substep labels for a single substep or unless it is a true sequence._

_For commands, introduce the command and then surround what the user must enter in the command prompt with three backticks, set the programming language if it applies, and follow with a pre attribute if you want the copy button applied._ For example:
Now you're ready to start working with the app. First, clone the repo with the sample app code.

   ```sh
   git clone https://github.com/IBM-Cloud/get-started-node
   ```
   {: pre}

   Then, change the directory to where the sample app is located.

   ```sh
   cd get-started-node
   ```
   {: pre}

_If you have any "tips" to include for this step, add the content in the flow where you would like it to appear. This information should be not be required information for completing the step, but helpful information in explaining additional concepts about what the user is doing in the step. It should be in the following format:_

One to two sentences of content that can include inline links or lists.
{: tip}

## Next steps
{: #anchor_value}

_What's the single thing the user needs to do next? Think "guided journey." Either provide information that leads the user to production use, for example HA, how to make a service secure, or how to connect to on-premise data. Or you can point the user to another tutorial. Give a choice between two options max._

### For all version dates
{: #02-june-2026-all-version-dates}

**Multiple algorithms for IKE and IPsec policies.** IKE and IPsec policies now support specifying multiple algorithms per category using array-based properties.

* **IKE policies:** Array-based properties `authentication_algorithms`, `dh_groups`, and `encryption_algorithms` are available. Existing singular properties (`authentication_algorithm`, `dh_group`, `encryption_algorithm`) are deprecated but remain supported. When [creating](/apidocs/vpc/latest#create-ike-policy) or [updating](/apidocs/vpc/latest#update-ike-policy) an IKE policy, you can specify either singular or array-based properties. PATCH operations allow mixing property types across categories. Responses for [retrieving](/apidocs/vpc/latest#get-ike-policy) or [listing](/apidocs/vpc/latest#list-ike-policies) include both property types using sentinel values (`"multiple"` for strings, `65535` for `dh_group`) if multiple algorithms are configured.

   Multiple algorithms are not supported for IKEv1 policies. IKEv1 policies are limited to a single algorithm per category. Array-based properties must contain only a single element per category.
   {: note}

* **IPsec policies:** Array-based properties `authentication_algorithms`, `encryption_algorithms`, and `pfs_groups` are available. Existing singular properties (`authentication_algorithm`, `encryption_algorithm`, `pfs`) are deprecated but remain supported. When [creating](/apidocs/vpc/latest#create-ipsec-policy) or [updating](/apidocs/vpc/latest#update-ipsec-policy) an IPsec policy, you can specify either singular or array-based properties. PATCH operations allow mixing property types across categories. Responses for [retrieving](/apidocs/vpc/latest#get-ipsec-policy) or [listing](/apidocs/vpc/latest#list-ipsec-policies) IKE policies include both property types.

Singular properties automatically update the corresponding array properties, and the reverse. This behavior differs from standard [JSON Merge Patch (RFC 7396)](https://datatracker.ietf.org/doc/html/rfc7396){: external} semantics where properties are updated independently.
{: note}
