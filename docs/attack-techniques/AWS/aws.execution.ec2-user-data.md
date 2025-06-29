---
title: Execute Commands on EC2 Instance via User Data
---

# Execute Commands on EC2 Instance via User Data

 <span class="smallcaps w3-badge w3-orange w3-round w3-text-sand" title="This attack technique might be slow to warm up or detonate">slow</span> 
 <span class="smallcaps w3-badge w3-blue w3-round w3-text-white" title="This attack technique can be detonated multiple times">idempotent</span> 

Platform: AWS

## Mappings

- MITRE ATT&CK
    - Execution
  - Privilege Escalation



## Description


Executes code on a Linux EC2 instance through User Data.

References:

- https://hackingthe.cloud/aws/exploitation/local-priv-esc-mod-instance-att/
- https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html

<span style="font-variant: small-caps;">Warm-up</span>:

- Create the prerequisite EC2 instance and VPC (takes a few minutes).

<span style="font-variant: small-caps;">Detonation</span>:

- Stop the instance
- Use ModifyInstanceAttribute to inject a malicious script in user data
- Start the instance
- Upon starting, the malicious script in user data is automatically executed as the root user


## Instructions

```bash title="Detonate with Stratus Red Team"
stratus detonate aws.execution.ec2-user-data
```
## Detection


Identify when the following sequence of CloudTrail events occur in a short period of time (e.g., < 1 hour)

1. <code>StopInstances</code> (necessary, because the user data of an instance cannot be changed when it's running)
2. <code>ModifyInstanceAttribute</code> with <code>requestParameters.userData</code> non-empty

When not possible to perform such correlation, alerting on the second event only is an option. It's generally not 
expected that the user data of an EC2 instance changes often, especially with the popularity of immutable machine images,
provisioned before instantiation.



## Detonation logs <span class="smallcaps w3-badge w3-light-green w3-round w3-text-sand">new!</span>

The following CloudTrail events are generated when this technique is detonated[^1]:


- `ec2:DescribeInstances`

- `ec2:ModifyInstanceAttribute`

- `ec2:StartInstances`

- `ec2:StopInstances`


??? "View raw detonation logs"

    ```json hl_lines="6 46 86 126 182 222 259 299 339 379 419 459 499 539 579 619 659 699 739 779 819 859 899 939 979 1019 1059 1099 1139 1179 1219 1259 1299 1339 1379"

    [
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "759fa0d5-d7d6-4de3-97f0-c469d1a0f92c",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:04:24Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "a9c78483-c047-4215-94c6-89794dd3b44e",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "914d32bb-067a-413c-adb1-cc8c4600261c",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:04:22Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "977121cb-f370-439d-9aa3-5dea3af27c6a",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "b38fe645-91d4-404b-8d64-024a6f7e00cd",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:04:20Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "fff5f8d6-d152-4d32-913e-a5fedaa6aa2f",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "55e470c0-611d-4549-ad87-a7c830a75063",
	      "eventName": "StartInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:04:19Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": false,
	      "recipientAccountId": "309303190113",
	      "requestID": "0c9bbf8a-a6f6-4e64-8396-78017a647f26",
	      "requestParameters": {
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": {
	         "instancesSet": {
	            "items": [
	               {
	                  "currentState": {
	                     "code": 0,
	                     "name": "pending"
	                  },
	                  "instanceId": "i-DDd6c7B0e18F0E35f",
	                  "previousState": {
	                     "code": 80,
	                     "name": "stopped"
	                  }
	               }
	            ]
	         },
	         "requestId": "0c9bbf8a-a6f6-4e64-8396-78017a647f26"
	      },
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "9e6d9e21-0c9c-49f7-b2b6-59c863d7a6a3",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:04:19Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "2ff3ad22-ffc2-4926-bbdd-15356ec9bd4a",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "f634894e-d625-4b7b-b1c1-50354cc1100e",
	      "eventName": "ModifyInstanceAttribute",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:04:18Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": false,
	      "recipientAccountId": "309303190113",
	      "requestID": "5c0d7f09-a80a-4313-b848-bc858fa4a8ad",
	      "requestParameters": {
	         "instanceId": "i-DDd6c7B0e18F0E35f",
	         "userData": "\u003csensitiveDataRemoved\u003e"
	      },
	      "responseElements": {
	         "_return": true,
	         "requestId": "5c0d7f09-a80a-4313-b848-bc858fa4a8ad"
	      },
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "8730ad3a-d87e-4463-aaba-d600442be64c",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:04:17Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "4ddaaecc-3c8d-420f-8646-977ad02fbbe5",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "50019cea-afa8-4dc4-b61d-b9454e6d2aba",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:04:16Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "277adb54-968d-4460-aeaa-a59d65139225",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "ae0d4f37-4d8c-49e1-ab78-2c7157ffc9d3",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:04:14Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "b38aa588-4cc4-4279-8117-2d1d06d8ff1f",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "daeb8d2a-a83b-4a37-8ba3-e60b3d0b69d1",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:04:11Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "e1ae237b-0241-4999-be50-44fd16f7e368",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "c751234f-ec7b-40d7-af60-188d8749b08f",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:04:10Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "398556fa-3fe5-4872-9d6f-a994e54731ed",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "bed3162f-6f64-4f6f-b08b-78d3ac9b9066",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:04:08Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "6e302813-c59e-49bc-ba23-89109cd64119",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "42d2c954-4b4c-4889-ad26-80796fe87025",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:04:06Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "8e3e3e2d-9593-442e-b8e5-335362f0a5df",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "2a1cbb02-88fd-4405-90f8-7d5bcb65b0f3",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:04:04Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "6d4d0e20-28c5-4bb0-90f2-57dfdc42aeab",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "56b0bf8c-92fe-460c-aaa6-ba5b9d816bea",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:04:03Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "a5ae54e8-dbcc-498c-ba6c-b7caff1d8302",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "5e44de78-52a2-4d5b-9b85-715f68110d00",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:04:00Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "6a7b7a28-eaa1-4a78-b7db-d5eb9b687773",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "7d0f96bf-ca3b-4bb6-b9ea-2cb20cbd3f64",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:03:58Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "21738dd4-cde5-4783-a4d9-341ffbb3d0f0",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "10469acd-d180-4b62-a768-15726f788cf6",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:03:57Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "cf1342c3-7142-4ce3-ace0-c3d6cb8ef53d",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "273a42f8-7c86-43f9-aabd-a698d0c5931a",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:03:55Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "3d41ca74-ae92-45de-ab0e-3c7ad6a38c24",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "b788f6c8-3155-4d3b-ac7d-9fd49e6be119",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:03:53Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "bf4fb83d-1fea-48c5-ab76-8914ce05ade1",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "8ced3c60-7e3a-447a-9abe-c80ea783e54a",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:03:51Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "2ff0ddac-4e87-448d-817e-5ec5e0d62ffa",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "933be44e-6ef0-44f6-a64b-99f067a71cd8",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:03:50Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "5bc0fc4e-a4fc-40b9-8a28-621a02c58e55",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "7eba0527-9926-4c43-8670-a4a1d2b8a466",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:03:48Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "6434fc93-d1b5-44f6-9d82-5323e1059b23",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "7a03fd83-ae64-41b4-b109-f672ccf01377",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:03:47Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "1758f71e-47d0-4fa3-9875-315bc7183bb3",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "e787e1ad-fa7c-4b91-9587-9beffd68488a",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:03:45Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "e9c76e24-ef65-4fdc-b30e-145643c6913a",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "40afc14c-3dd8-4195-b4d3-89f1173d368f",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:03:43Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "ed3889da-12fc-434b-8e5d-5bcf122b46fe",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "8bc46582-5202-4857-879e-b57a94862895",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:03:41Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "1b8980f2-0a5e-4e6a-8a5a-82a4982d4a36",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "7470d5b5-0e71-4bd2-9809-8b8e9499b8e2",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:03:40Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "2d9bbbbf-86ab-4e36-8f44-66b9cc568571",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "570ab1e6-8222-4db2-a688-6c1a37cc9968",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:03:38Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "33647962-fb50-4bc9-9465-13d237860e4f",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "9ed4f1c7-607c-4c88-bcb6-053a03fd30cc",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:03:37Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "88449386-205f-4091-b667-5b9efc5ce256",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "7c46e00c-5eba-40c4-8a5c-3788c10af6fd",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:03:35Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "40f0177a-b1a4-44a4-b6c5-87fd9e44849e",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "4edfbd95-32ab-4abc-9b07-5e371a9af5da",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:03:34Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "cc43b6de-04d9-4435-9ecc-46a575b0950d",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "8ffd8499-55e5-4487-b1c8-f73ab389db84",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:03:32Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "0a967e8c-b6ed-4870-aec5-edca45b2e00c",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "bfdbd679-9ac4-41e0-84f6-2be3ac12d3e5",
	      "eventName": "DescribeInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:03:30Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "309303190113",
	      "requestID": "14975c6a-e0f8-4abf-b731-5a21a8249464",
	      "requestParameters": {
	         "filterSet": {},
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": null,
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "ca-northsouth-2r",
	      "eventCategory": "Management",
	      "eventID": "d373b5dd-6a82-439d-bdcf-4e6c7c7a9292",
	      "eventName": "StopInstances",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-08-01T12:03:30Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": false,
	      "recipientAccountId": "309303190113",
	      "requestID": "088dba72-717e-4502-a3c5-5c95f22f87c1",
	      "requestParameters": {
	         "force": true,
	         "instancesSet": {
	            "items": [
	               {
	                  "instanceId": "i-DDd6c7B0e18F0E35f"
	               }
	            ]
	         }
	      },
	      "responseElements": {
	         "instancesSet": {
	            "items": [
	               {
	                  "currentState": {
	                     "code": 64,
	                     "name": "stopping"
	                  },
	                  "instanceId": "i-DDd6c7B0e18F0E35f",
	                  "previousState": {
	                     "code": 16,
	                     "name": "running"
	                  }
	               }
	            ]
	         },
	         "requestId": "088dba72-717e-4502-a3c5-5c95f22f87c1"
	      },
	      "sourceIPAddress": "251.228.255.218",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.ca-northsouth-2r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_54d79918-8729-4201-83e6-6a600173b8e3",
	      "userIdentity": {
	         "accessKeyId": "AKIAZI86ACIZ2J9CV86Z",
	         "accountId": "309303190113",
	         "arn": "arn:aws:iam::309303190113:user/christophe",
	         "principalId": "AIDAV0KQ3LIBUIGZ52WB",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   }
	]
    ```

[^1]: These logs have been gathered from a real detonation of this technique in a test environment using [Grimoire](https://github.com/DataDog/grimoire), and anonymized using [LogLicker](https://github.com/Permiso-io-tools/LogLicker).
