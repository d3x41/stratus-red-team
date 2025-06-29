---
title: Attempt to Leave the AWS Organization
---

# Attempt to Leave the AWS Organization


 <span class="smallcaps w3-badge w3-blue w3-round w3-text-white" title="This attack technique can be detonated multiple times">idempotent</span> 

Platform: AWS

## Mappings

- MITRE ATT&CK
    - Defense Evasion


- Threat Technique Catalog for AWS:
  
    - [Modify Cloud Resource Hierarchy: Leave AWS Organization](https://aws-samples.github.io/threat-technique-catalog-for-aws/Techniques/T1666.A002.html) (T1666.A002)
  


## Description


Attempts to leave the AWS Organization (unsuccessfully - will hit an AccessDenied error). 
Security configurations are often defined at the organization level (GuardDuty, SecurityHub, CloudTrail...). 
Leaving the organization can disrupt or totally shut down these controls.


<span style="font-variant: small-caps;">Warm-up</span>: 

- Create an IAM role without permissions to run organizations:LeaveOrganization

<span style="font-variant: small-caps;">Detonation</span>: 

- Call organization:LeaveOrganization to simulate an attempt to leave the AWS Organization.


## Instructions

```bash title="Detonate with Stratus Red Team"
stratus detonate aws.defense-evasion.organizations-leave
```
## Detection


Any attempts from a child account to leave its AWS Organization should be considered suspicious. 

Use the CloudTrail event <code>LeaveOrganization</code>.


## Detonation logs <span class="smallcaps w3-badge w3-light-green w3-round w3-text-sand">new!</span>

The following CloudTrail events are generated when this technique is detonated[^1]:


- `organizations:LeaveOrganization`

- `sts:AssumeRole`


??? "View raw detonation logs"

    ```json hl_lines="6 60 103"

    [
	   {
	      "awsRegion": "euiso-south-3r",
	      "eventCategory": "Management",
	      "eventID": "099bfd30-232c-4dff-9998-3821921063ca",
	      "eventName": "AssumeRole",
	      "eventSource": "sts.amazonaws.com",
	      "eventTime": "2024-08-02T08:30:00Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "307578594326",
	      "requestID": "4ddeba69-b9da-48b8-833a-c4d75f10111e",
	      "requestParameters": {
	         "durationSeconds": 900,
	         "roleArn": "arn:aws:iam::307578594326:role/stratus-red-team-leave-org-role",
	         "roleSessionName": "aws-go-sdk-1722587398902687000"
	      },
	      "resources": [
	         {
	            "ARN": "arn:aws:iam::307578594326:role/stratus-red-team-leave-org-role",
	            "accountId": "307578594326",
	            "type": "AWS::IAM::Role"
	         }
	      ],
	      "responseElements": {
	         "assumedRoleUser": {
	            "arn": "arn:aws:sts::307578594326:assumed-role/stratus-red-team-leave-org-role/aws-go-sdk-1722587398902687000",
	            "assumedRoleId": "AROAHKPEEQ9BHUOX4D93T:aws-go-sdk-1722587398902687000"
	         },
	         "credentials": {
	            "accessKeyId": "ASIA36EV31F1RB3OA8IG",
	            "expiration": "Aug 2, 2024, 8:45:00 AM",
	            "sessionToken": "IQoJb3JpZ2luX2VjEKH//////////wEaCXVzLWVhc3QtMSJIMEYCIQDpLWjFwlZiDhOSv0Wy5mFb/hiIvKvEuUFZxT+drWNuGQIhAN4f3+HUyrPT31KHNwCaurNZJk5wXAWdp3sNeX03lBnkKrQCCIr//////////wEQARoMNzUxMzUzMDQxMzEwIgw0GyjMjlRuPkefn0kqiALzs49PG+DYSQ2tdUjlPv3YOOCAHkwaO7GpRcy9Yjo7R5RHGBw7NQSJmTGSahF/InmScEUvU8BV+ZuCsQJew5QC7yNqD4FNV5gCdj/0r0w/rh+GITYgoUvv47Xz4cGIhKfk0lsQiYptWeWq2htiubjbemAz2YXxoZUCHkGQUm1taei/jRfdMANUc63J20JxrhURAh2p3A/Aw9syW/eGT1AtV5Va4BpKkA/ik5mya7eEuyMxuXgldHBIXV04+7OcnJbkqJE+RMrP29VY5Z01ajs3NXTEFKpawctP8LXUIkfQDA35gSloJkeWlY4NUuN9rigCMjwRg1WpMWeQy/LwSm/BUm8mSh+dPYwwiLKytQY6nAFRnnDovTfsWAjH7oSq4aJzURd0sd5LeU0vbOMPSVXao0aCCZsF3kYLiZoy1vpfukI/z+FmduLMW1RpKR2S0iC0LUUrRekErl46GBRqnuc6T05CFZx9sFr0rUnTQlCJQFznQP2WLiQWs1uBL6vxrOi8bTuq4kTgOFJfGRIar0CXOOO19fPf5TDZ1HKRFo55VcamguEk4L4DCAJLivM="
	         }
	      },
	      "sourceIPAddress": "252.5.222.230",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "sts.euiso-south-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_fd969928-3c0d-4feb-bd56-34f9aee3e6eb",
	      "userIdentity": {
	         "accessKeyId": "AKIADVISM0T50G52IF0D",
	         "accountId": "307578594326",
	         "arn": "arn:aws:iam::307578594326:user/christophe",
	         "principalId": "AIDA7YYMW5FLWE3HGTNZ",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "euiso-south-3r",
	      "errorCode": "AccessDenied",
	      "errorMessage": "User: arn:aws:sts::307578594326:assumed-role/stratus-red-team-leave-org-role/aws-go-sdk-1722587398902687000 is not authorized to perform: organizations:LeaveOrganization on resource: * because no identity-based policy allows the organizations:LeaveOrganization action",
	      "eventCategory": "Management",
	      "eventID": "16903cbd-fdff-4818-82f2-d66ad09aaf57",
	      "eventName": "LeaveOrganization",
	      "eventSource": "organizations.amazonaws.com",
	      "eventTime": "2024-08-02T08:30:00Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": false,
	      "recipientAccountId": "307578594326",
	      "requestID": "47bd7f8f-1cbf-49df-8503-7d60917e721a",
	      "requestParameters": null,
	      "responseElements": null,
	      "sourceIPAddress": "252.5.222.230",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "organizations.euiso-south-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_fd969928-3c0d-4feb-bd56-34f9aee3e6eb",
	      "userIdentity": {
	         "accessKeyId": "ASIA36EV31F1RB3OA8IG",
	         "accountId": "307578594326",
	         "arn": "arn:aws:sts::307578594326:assumed-role/stratus-red-team-leave-org-role/aws-go-sdk-1722587398902687000",
	         "principalId": "AROAHKPEEQ9BHUOX4D93T:aws-go-sdk-1722587398902687000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-08-02T08:30:00Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "307578594326",
	               "arn": "arn:aws:iam::307578594326:role/stratus-red-team-leave-org-role",
	               "principalId": "AROAHKPEEQ9BHUOX4D93T",
	               "type": "Role",
	               "userName": "stratus-red-team-leave-org-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "euiso-south-3r",
	      "eventCategory": "Management",
	      "eventID": "e3441619-0bf6-4818-bf18-391fb65ba98e",
	      "eventName": "AssumeRole",
	      "eventSource": "sts.amazonaws.com",
	      "eventTime": "2024-08-02T08:29:59Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "307578594326",
	      "requestID": "0af9d3b8-6911-407f-a3e7-b54c4e36e41c",
	      "requestParameters": {
	         "durationSeconds": 900,
	         "roleArn": "arn:aws:iam::307578594326:role/stratus-red-team-leave-org-role",
	         "roleSessionName": "aws-go-sdk-1722587398902687000"
	      },
	      "resources": [
	         {
	            "ARN": "arn:aws:iam::307578594326:role/stratus-red-team-leave-org-role",
	            "accountId": "307578594326",
	            "type": "AWS::IAM::Role"
	         }
	      ],
	      "responseElements": {
	         "assumedRoleUser": {
	            "arn": "arn:aws:sts::307578594326:assumed-role/stratus-red-team-leave-org-role/aws-go-sdk-1722587398902687000",
	            "assumedRoleId": "AROAHKPEEQ9BHUOX4D93T:aws-go-sdk-1722587398902687000"
	         },
	         "credentials": {
	            "accessKeyId": "ASIAMOWPWQJ1QHWCWJXJ",
	            "expiration": "Aug 2, 2024, 8:44:59 AM",
	            "sessionToken": "IQoJb3JpZ2luX2VjEKH//////////wEaCXVzLWVhc3QtMSJHMEUCIEn2jTjXoiKEo1nDM8a/bLpChCnNR5DiuhZ/X7Nb+LPgAiEAnbcwRa2KudpyvlCk/Rp1BejOkEXlpQzJoLaMyfhQpq0qtAIIiv//////////ARABGgw3NTEzNTMwNDEzMTAiDER+6/kn5hAd98DsoCqIAmEmhie9s2iLhj9Nf3lGI2Ezprwy/Zk/HQRQPKuxJu6+0ZyRwAlgZeXcOTfjo3xTdiVRTNiu9SUOAFNsMoiIvVFOofY0XojtNMVKA1PVNjcDqpidgdJZGeFMnGXSEb5ea4ZLUCY6sOm4SgsL2vuPOz5i+Bz40ajwu5bAfNnrXFnPHqwLQnf0PSCZQmbESeo0KjQ7TQ0Vw3mjWP2aW0EJFw789hyQthYLkQPoZrqw9n3FCnX7IidusIVIAjOVh4Da3aw8nWhiwOEizs9UX0ZIq+wmeWx6B4MuzMCp9BNNRGqxhO4Mje2K+Z3qd1+6RC/AdydJwHuoNi0oAY0t1yFb4DyzQyD9Gi3qXzCHsrK1BjqdAU23Sc9g9h/uJPJIB81GJ2hEqAToB/tYMJSINsK9vbSLa3ugqzTo9AD3Y95d3jVv7VB1bKIX2FMhcTqKpKZKtmriqAZJ3UNgNA9ZMf31H35M87SXbVN0z2a9H8XZO4iQrdNQzKBR8rlGOb6i+UefrltFQLdRbwKbfiWsiZwkEyz5RK7794ELHV+3328jn+GUiJYTv731tRSkrc7wfvE="
	         }
	      },
	      "sourceIPAddress": "252.5.222.230",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "sts.euiso-south-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_fd969928-3c0d-4feb-bd56-34f9aee3e6eb",
	      "userIdentity": {
	         "accessKeyId": "AKIADVISM0T50G52IF0D",
	         "accountId": "307578594326",
	         "arn": "arn:aws:iam::307578594326:user/christophe",
	         "principalId": "AIDA7YYMW5FLWE3HGTNZ",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   }
	]
    ```

[^1]: These logs have been gathered from a real detonation of this technique in a test environment using [Grimoire](https://github.com/DataDog/grimoire), and anonymized using [LogLicker](https://github.com/Permiso-io-tools/LogLicker).
