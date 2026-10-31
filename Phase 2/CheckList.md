## Phase 2
Your Security Recommendations You’re an M365 security consultant to small and medium sized businesses with 100 users, two admins, and Microsoft 365 E7 licenses. Recommend and rank the top five M365 security controls they should implement. For each control: Explain why it matters and the risk it mitigates. Describe how you’d roll it out with limited admin resources. Identify key drawbacks or tradeoffs

## First Checkup

MFA: Apply this to all users in the organization. Raise awareness about the use of phishing-resistant authentication methods, such as passkeys, among administrators and senior staff (managers). This control is important because it prevents accounts from being compromised by brute-force attacks, such as token theft.

It would be added as an additional authentication method.

*_Figure 1: Conditional Access policy settings for MFA for all users. Exclude emergency accounts._*
![image alt](https://github.com/ecoronell33/Defender-the-Tenant-Challenge/blob/2d0d900b6ef31d1c415e5636c8487c32329c6382/Phase%202/Policy_ALL_MFA.png)


*_Figure 2: Enforce the use of a phishing-resistant authentication method._*
![image alt](https://github.com/ecoronell33/Defender-the-Tenant-Challenge/blob/d4c78bb35e554035e91e50853162a09f4569c36c/Phase%202/PR-MFA.png)

Most customers need a service account for sending via SMTP, where the applications are legacy systems that do not support MFA requests. Therefore, to resolve this, we will use the SMTP relay method through a connector, specifying the public IP address from which the emails are sent.

* It is recommended to add the public IP address as an additional record to the SPF record.

*_Flow: The process of sending messages via an SMTP relay._*
  ![image alt](https://github.com/ecoronell33/Defender-the-Tenant-Challenge/blob/ff908ef9f4643341c9027f685cba3afaaa1960fb/Phase%202/SMTPRelay.png)

## Second Checkup

Mail: Configure the SPF, DKIM, and DMARC email authentication protocols. This is essential for reducing phishing emails, which are often a major attack vector for Microsoft 365 users. 

**SPF**

Applications and/or SMTP servers authorized to send emails based on the domain

```powershell
sharkdump.org.pe 3600 EN TXT "v=spf1 include:spf.protection.outlook.com -all"
```

**DKIM**

Add a digital signature to every outgoing email. This helps prevent information from being altered in transit and/or verify ownership.

```powershell
selector1._domainkey 3600 EN CNAME selector1-sharkdump-org-pe._domainkey.lantester.w-v1.dkim.mail.microsoft

selector2._domainkey 3600 EN CNAME selector2-sharkdump-org-pe._domainkey.lantester.w-v1.dkim.mail.microsoft
```

**DMARC**

It monitors the DKIM and SPF filter statuses; if either fails, it follows the established procedure for rejection or quarantine. In addition, DMARC verifies that the MAILFROM (the MTA from which the email is sent) matches the FROM domain to reduce instances of identity spoofing.

```powershell
_dmarc.microsoft.com. 3600 EN TXT "v=DMARC1; p=reject; rua=mailto:amedina@sharkdump.org.pe; ruf=amedina@sharkdump.org.pe; fo=1 
```

## Parameters

- "v=DMARC1: DMARC version

- p=reject: Type of action the receiving server will take if DKIM and SPF fail

- rua=mailto:d@rua.contoso.com: Specifies the senders who will receive a mail traffic report

- fo=1: requires receiving servers to send a failure report if DKIM or SPF fails; these reports will be sent to the senders specified in the RUA or RUF fields

**In addition, Microsoft Defender configures a strict policy by default for users in roles such as executives or CEOs, who are more likely to be targeted by impersonation or phishing attacks.**

**When a strict, predefined policy is established, the filters may flag legitimate emails as false positives. To prevent this, users will be able to release these emails through a custom quarantine policy.**

*_Preset security policies: Strict protection._*
![image alt](https://github.com/ecoronell33/Defender-the-Tenant-Challenge/blob/150888eeb53bda15d186d5d01324007d1d136fc2/Phase%202/Strict%20Policies.png)

## Third control

Device:  Configure the default compliance policy to identify devices that do not meet the requirements, along with a conditional access policy. This prevents access to Microsoft 365 applications from untrusted devices. This prevents access to each user’s confidential information even if their account were to be compromised, since it will only allow sign-in from devices that meet certain compliance settings.

*_Compliance Settings._*
![image alt](https://github.com/ecoronell33/Defender-the-Tenant-Challenge/blob/f0aeff2ea038718da8aafb925d0a096ea96b6d12/Phase%202/Compliance%20settings.png)


**Caution is advised if this is disabled in production; therefore, we recommend creating an inventory of devices to identify which ones need to be added to a compliance policy to prevent service disruption.**

## Fourth Control

Data: Apply confidentiality labels to sensitive data, such as emails and organizational documents, to prevent information loss and/or unauthorized access to such data. 
This measure will allow certain content to be categorized and encrypted so that it can be accessed only by authorized individuals.

*_Sensitivity labels._*
![image alt](https://github.com/ecoronell33/Defender-the-Tenant-Challenge/blob/53b472d1c0214de0526cad2f130723f7def8467f/Phase%202/Sensitivity%20labels.png)


## Fifth Control

Apps: Restrict SaaS application consent to the administrator. This control is important because it prevents users from granting consent and/or accepting certain OAuth permissions to malicious applications

*_Consent apps._*
![image alt](https://github.com/ecoronell33/Defender-the-Tenant-Challenge/blob/91f752451b1dd41cf6028cc4e7aa9c4a6028d4a1/Phase%202/Consent_and_permissions.png)

