---
pcx_content_type: how-to
title: AWS IAM (SAML)
weight: 5
---

# AWS IAM (SAML)

AWS IAM Identity Center provides SSO identity management for users who interact with AWS resources (such as EC2 instances or S3 buckets). You can integrate AWS IAM with Cloudflare Zero Trust as a SAML identity provider, which allows users to authenticate to Zero Trust using their AWS credentials.

## Prerequisites

- Admin access to an IAM Identity Center [organization instance](https://docs.aws.amazon.com/singlesignon/latest/userguide/identity-center-instances.html)

## Set up AWS IAM as a SAML provider

To set up SAML with AWS IAM as your identity provider:

1. Open your [IAM Identity Center console](https://console.aws.amazon.com/singlesignon) and go to **Applications**.

2. Select the **Customer managed** tab.

3. Select **Add application**.

4. Select **I have an application I want to set up**.

5. For **Application type**, select **SAML 2.0**.

6. Select **Next**.

7. Enter a **Display name** for the application (for example, `Cloudflare Zero Trust`).

8. Download the **IAM Identity Center SAML metadata file**. You will need this file later when configuring the identity provider in Zero Trust.

9. Under **Application metadata**, select **Manually type your metadata values**.

10. In **Application ACS URL** and **Application SAML audience**, enter the following URL:

   ```txt
   https://<your-team-name>.cloudflareaccess.com/cdn-cgi/access/callback
   ```

  You can find your team name in Zero Trust under **Settings** > **Custom Pages**.

11. Select **Submit**.

12. Next, select the **Actions** dropdown menu and select _Edit attribute mappings_.

13. For the `Subject` user attribute, enter `${user:email}`.

14. (Recommended) Add user name attributes:

| User attribute | String value |
| -------------- | ------------ |
| `name` | `${user:name}` |
| `surName` | `${user:familyName}` |
| `givenName` | `${user:givenName}` |

![Configuring attribute statements in IAM Identity Center](/images/cloudflare-one/identity/aws/aws-saml-attributes.png)

15. Select **Save changes**.

16. Under **Assign users and groups**, add individuals and/or groups that should be allowed to login to Zero Trust.

17. In [Zero Trust](https://one.dash.cloudflare.com), go to **Settings** > **Authentication**.

18. Under **Login Methods**, select **Add new**.

19. Select **SAML**.

20. Enter a **Name** for the IdP integration (for example, `AWS`).

21. Upload the **IAM Identity Center SAML metadata file** that you downloaded in Step 8.

22. (Recommended) Enable [**Sign SAML authentication request**](/cloudflare-one/identity/idp-integration/generic-saml/#sign-saml-authentication-request).

23. Select **Save**.

To [test](/cloudflare-one/identity/idp-integration/#test-idps-in-zero-trust) that your connection is working, select **Test**.

## Example API configuration

```json
{
  "config": {
      "issuer_url": "https://portal.sso.eu-central-1.amazonaws.com/saml/assertion/b2yJrC4kjy3ZAS0a2SeDJj74ebEAxozPfiURId0aQsal3",
      "sso_target_url": "https://portal.sso.eu-central-1.amazonaws.com/saml/assertion/b2yJrC4kjy3ZAS0a2SeDJj74ebEAxozPfiURId0aQsal3",
      "attributes": [
          "email"
      ],
      "email_attribute_name": "email",
      "sign_request": true,
      "idp_public_certs": [
          "MIIDpDCCAoygAwIBAgIGAV2ka+55MA0GCSqGSIb3DQEBCwUAMIGSMQswCQYDVQQGEwJVUzETMBEG\nA1UEC.....GF/Q2/MHadws97cZg\nuTnQyuOqPuHbnN83d/2l1NSYKCbHt24o"
      ]
  },
  "type": "saml",
  "name": "AWS IAM SAML example"
}
```
