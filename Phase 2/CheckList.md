## Phase 2
Your Security Recommendations You’re an M365 security consultant to small and medium sized businesses with 100 users, two admins, and Microsoft 365 E7 licenses. Recommend and rank the top five M365 security controls they should implement. For each control: Explain why it matters and the risk it mitigates. Describe how you’d roll it out with limited admin resources. Identify key drawbacks or tradeoffs

## First Checkup

1) MFA: Apply this to all users in the organization. Raise awareness about the use of phishing-resistant authentication methods, such as passkeys, among administrators and senior staff (managers). This control is important because it prevents accounts from being compromised by brute-force attacks, such as token theft.

It would be added as an additional authentication method.

Most customers need a service account for sending via SMTP, where the applications are legacy systems that do not support MFA requests. Therefore, to resolve this, we will use the SMTP relay method through a connector, specifying the public IP address from which the emails are sent.

* It is recommended to add the public IP address as an additional record to the SPF record.

  ![image alt](https://github.com/ecoronell33/Defender-the-Tenant-Challenge/blob/ff908ef9f4643341c9027f685cba3afaaa1960fb/Phase%202/SMTPRelay.png)

3) Mail: Configure the SPF, DKIM, and DMARC email authentication protocols. This is essential for reducing phishing emails, which are often a major attack vector for Microsoft 365 users. 

4) Device:  Implement a compliance policy that identifies non-compliant devices, along with a conditional access policy. This prevents access to Microsoft 365 applications from untrusted devices and reduces data theft.

5) Data: Apply confidentiality labels to sensitive data, such as emails and organizational documents, to prevent information loss and/or unauthorized access to such data. 

6) Apps: Restrict SaaS application consent to the administrator. This control is important because it prevents users from granting certain OAuth permissions to malicious applications.



