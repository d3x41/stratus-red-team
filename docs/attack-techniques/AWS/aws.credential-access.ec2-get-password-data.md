---
title: Retrieve EC2 Password Data
---

# Retrieve EC2 Password Data


 <span class="smallcaps w3-badge w3-blue w3-round w3-text-white" title="This attack technique can be detonated multiple times">idempotent</span> 

Platform: AWS

## Mappings

- MITRE ATT&CK
    - Credential Access



## Description


Runs ec2:GetPasswordData from a role that does not have permission to do so. This simulates an attacker attempting to
retrieve RDP passwords on a high number of Windows EC2 instances.

See https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_GetPasswordData.html

<span style="font-variant: small-caps;">Warm-up</span>: 

- Create an IAM role without permissions to run ec2:GetPasswordData

<span style="font-variant: small-caps;">Detonation</span>: 

- Assume the role 
- Run a number of ec2:GetPasswordData calls (which will be denied) using fictitious instance IDs


## Instructions

```bash title="Detonate with Stratus Red Team"
stratus detonate aws.credential-access.ec2-get-password-data
```
## Detection

Identify principals making a large number of ec2:GetPasswordData calls, using CloudTrail's GetPasswordData event


## Detonation logs <span class="smallcaps w3-badge w3-light-green w3-round w3-text-sand">new!</span>

The following CloudTrail events are generated when this technique is detonated[^1]:


- `ec2:GetPasswordData`

- `sts:AssumeRole`


??? "View raw detonation logs"

    ```json hl_lines="8 55 102 149 196 243 290 337 384 431 478 525 572 619 666 713 760 807 854 901 948 995 1042 1089 1136 1183 1230 1277 1324 1371 1416 1468 1522 1555"

    [
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::751353041310:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:751353041310:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: fqhg8CzmasrUP43_LGsSmLVAAoKKY1CzQD3yqWpWiuZGOcVf2lhbhrrgsH8zy44fLcyyL6AsNcXA2GMJ3dl_2A8-mR5qE3oPDbM8k51n_qGm4fs4CdzuYK01dKPn4abyT2RXgAphwvURW0X-7R1OFTrWQnRH_W-pWiKQMJ756fS410A5yi504958O5VwFgOoxzBqwSFmvPt5WRVqBpuxTA_CXq5ogP2bjZzdHV8g_FnbHOARLP282lJjyBlNgP09SyB40bDDBxwDhYm_57waaVMA1Ww-_SlUt02HzVBZp7t7ta8udTCpZsoNuZyhUPmgli8z1pwkKVbsVe1cEhokOPPDm3p5ymcSZ4o5mwtEk18p46uE1SHVZSUv23Pjv68qZe0Sj_-rLKzqTi4Mhje-h5a7zRf8i3P-LGTGJHUxH4y5C2e659kdVhTaUJv8maLCMDiL7cUX2Px3xCyiWvtAnA_NIpmXEboFADuVzUsVVl-sTdCTT1rZn_-ts_xbdrqSmzvGKsDiTB1vJF3UwFjRuSRVSPD0g_U_rkZfqy0j-JEUU3DEIsh4SIWsrgDNuPzv0KQ",
	      "eventCategory": "Management",
	      "eventID": "450230d4-b39e-4a18-a6a0-d07a6e2105cb",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:21Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "b20c2df5-71d5-441e-84c8-b424f1c78ffb",
	      "requestParameters": {
	         "instanceId": "i-i2jnm5swa59p4fxg"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: vI3cDVgKJvmlMzN8rT24DeQOh9di8wn6vWRhl7MKZYEHwshGC7bY0RXqvxRIFTQNaddFRU7snsmuRbDWCJhQ5b_E7tu5T614NYSVWVA-voW06n-BOfulZtczb3PyUhqbGpg9vjiiY-OrpAWZ6F025pam2NYdRGvNYxLxrRIJcc-Pgy6AOKrgqoBuIYS9KWg1xhnVaU_MwL79F31AiLn_2xPKnBmuxw0Gbf66kSPQi4HBkBT7hpsCLz9iyrVLOOGUV8yKQM95ZzvoGL0hxfMCiLL1PxQAkAECTuhYIMseN7dDrkwqyy5CUjQmKCmKxJvwskEp5WZogiQjtkk44pe-ODMesOjJx5jGfWhpbpXS505jUD5noJpQtzF3HTuCecAdsUezzqJMy7xfgKfZwM_0S5vxuP71ZdLGUIyI8dXT8yyGvVdennbqgGnmSlgR5236yhxAsYtX7mRP5-pNjVGsPvz0YOA0MYzyQHTAmHFqsMK3efkeySF4DqsrvFp8E-_4zQuOy8xcsl2Lt0EXibfAqUOwRxh1n0TZ5hJ3_KgirWcFGhfAEDlgK_btXALP9uWvgAA",
	      "eventCategory": "Management",
	      "eventID": "560bcc37-36b9-43f4-8447-2bab2d7cd7cf",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:21Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "b25e0f2a-0a98-4b8f-8893-fce249e28a83",
	      "requestParameters": {
	         "instanceId": "i-aq9pmsueolxr81r5"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: MvyJ8E7JRlJ8qrNOLCxvOgNHlEoVtWB6q7tZDACABTX_jUO8rHfwdhptvxZXjjrECMntJyC781EvTNGomFMVEsi7X7m3WYsdVSCTV3_b6vvnO73HHYOPDJA67Uu860JC_nvDqubgE8tVYaEQfIv2tkoLOa_giq3CnHTnT8OTem2osy1fvZ9ZoqtOm8L_yt0o_Xa4gm1q4uhq_9OjanBPHK1Vi1EKlOSAu6MMD6_QHoby_vZMs8zBqXHZMMZKh7ENCR-RVW-nutH3WyZ9kUyKK9ZoLCD4RKh7OR9xuvs6b5p-SvvIhC9W4SYFhSUcbqXr32IDoY0T6IaaYY_I-ZBxJJv8sDWP4FFx-Zgnj6jkJwbpJL3zrDF5t1uYx_-d7dl7fXztnlaSFchdmdBtu2gWlakT8vwWFKIAWFlP9EzDVsooEN8jBT9CT7XasorGDrjMkoXUL74wSQ8bsbZuXazBBT3xK2cfXoCZQ_YYW1ITOif_RAHKzn78evQrg917qNktjM09reyr9xYP34rMbKlabtbZwx0KKP8xtSU_teXhTMRQ5UydA9NQMCCGvrjd2-TWdaM",
	      "eventCategory": "Management",
	      "eventID": "76a3f52e-5c4c-4a62-818d-a2bc8bddc2e6",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:21Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "f48594da-0a0d-4e9c-a641-0f9dd4fec8fc",
	      "requestParameters": {
	         "instanceId": "i-x7jwh6qy39glvq1r"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: ex0NeuPRe8xwBXWSB-bMPAP_IMRYGNBpDD0SaeD3RV2Y-0w39P_2oAFjmi-r8BNT69RYOJ-hza1FZen-cwGssTUW5prEYz1Nf1c1nmupsXlbIS9oGexXcLlk0eftjhtp1oW5mxnhE0QYe_1VvGLde6mv5FsTKvO8_kcW0HuKi47kTgBB1RlLnjXrBQ9D6bUqmpyJzPv-9R651JtTJ0dggDS7lEN0vagJI1y7MdhgUnr63ZFDwwNN9tHzZS_jzC232IH5Nh-4AFSvPYYcHP75ahrQBARAriMWycPyvQZypwEwR5IeM9pDwnVPbhQZnk07KV67c-B5Y_VIv0rmaSpCsf0HEwW5kCP1QV6CZIpnCTku1Ghwt-nCouj_Yv62oJg3j8xTBMgivye_UC_mv2zDF9vCcsWQ7F2-uit-rbKyzIKC72UBP5DAchNYeHhBShD9heqssLqgNrpO_1nTzA_bUdxWiVCI20QRazEobNiVm9vbdDB_LD9mLpvfQsT8k8qWT1_E7yaR9_1ZVcW13BZ2zDD66YBIIiKD3bVixCibVF1VuktZcM0DMHYquWHyJyqN_o5L",
	      "eventCategory": "Management",
	      "eventID": "0c7f6148-c337-4e49-8df5-cb333c6fb7d6",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:20Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "4fa792f1-a997-4739-a79c-215983a2cce7",
	      "requestParameters": {
	         "instanceId": "i-vgu76uxucxlpp04e"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: sy7SXIS8cR0ggyin7T9E00rq0UiBYf3eugsTZ-Ogk79Vr7gPWzUxv5S1-6UGbgDluSzgK5qh5bj2VmJiWaAwIlMfWlkTKGSQkcf5gz5wOK7xVi-QjG_ZZMg6JlpeQlf42ElPwTHSlsjHU7OIRcFmIpSy15svaRMouoxwxKfdDF7FtruzOBMlbwFSS9EjcO9BS_SHVSsJte6TxSYwyrR4tNVke6T_P4rBeL7ztd7h_W5CInqYvgQV8ivmmB3ZCKHmui3eS5NaWAlVPYiPUIv5h2VUjqzEt3HsSHpjdQQuXOoSy3lQuqGNgSBwMuemwkT1hcpmSyUWkdKbIuVMHGKvPx5fh5SBkcIUEn4Zijtlo6qWX9q_A739rbuQs9Tek1i1N5xO5f0ab_sepQdNEQZexx8lT8H8lOwjPZNrcUuppHp2o3sbVJgMn-75snd68YVWP3u0-QuNiQ-TyBYuu-RCVOct_7dOhDEwIixzMKgX-xbSm0AMICAT5saVXRwwrL1PB63t2nq52lWHstgzS5hapqr8GBhT6VHgjiPgadckQde1p8cN476Y_3nt4vbjTlixyHQ",
	      "eventCategory": "Management",
	      "eventID": "4bf1ca5d-42ba-4e95-a493-9cdeefb58b87",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:20Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "8efcec3b-8c76-4b8b-acc4-884b7040aa69",
	      "requestParameters": {
	         "instanceId": "i-ozzfav7qglzosg49"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: w4BeNvjyqgZy54yIPW-Fi1znuurlnMJBtXRoh5NdfY7bT8fFvjHYaLQ6EUXTTjnEMB4Gv5bwqpgFzM5lzvWFweErUq6l2N5nvU_e2hVJgAhQyDII36qsr2Jj_XeFX6UoQb3pimMn6T4q4oDxP7FtsIt8uIrAVxc5ECs_3JbDgshdjVHf0yz0VgZprSF-2bbppKqgD_B1BkIEe587cUlDyrH6XszhIww2-k6Jj82FrDBowlBEJwREI9VnJdFWFO5y1NInklHF_bBFkyat2Nr5aXpwDUMEPY6dY5Ggv2I2ggujHKbtkXRF4AbxCN1SfyX3jLS98ewC3mZaVymcADN1KRghMytqsxMfjAeOOi0OzUrLZl5YcWCN9cH1Sca2KU5ZISpwGQSETyCD--KM5_J8mHQS_ijmTXUXxCpdjgUZRo3dn4Krll1H18IlRMtovF5KqR4HpPL4bVX1l6LL8e2gs3x_NtQys8aWA1aybnT6dWP12eb7P_j6YKziDMfp6zx1smQjHlPwxRg3I7w84EcpCXdNIpqVSxOo-PrmpH5u_0rfkHXEzjfYX5vbJ-dt8BeDOfA",
	      "eventCategory": "Management",
	      "eventID": "7466f497-7987-44d9-aeb1-5034d02c9f87",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:20Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "9da09dec-5398-47cf-a763-ebab997f543f",
	      "requestParameters": {
	         "instanceId": "i-t3wz01wvchd1i3ji"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: D8EVYsR5r16Iqx5IHuCEN7fghFzk7W_8XbwrZzPIH0vwpygIn9k9LSeOsmINlF6dZU9r9rWXxbpxmmnwr39FJS7UAyqkNvN-nMQc-ySOHrTZobFllAx1vwRNnYVUwu_AMKV6ov2s-969CBXV4OImXntzJmBLx_lsvb27jey_rQLzS-1H8hpXoQl2lKsBr4NZNk7xUEpPs_5a6V-ZkPBA_UoTXn6xIBmjC5y_gNwvWeP-OpTa6hmG-XKsPGrr5zP-b07P0gkc6k9ykR7e2MTQ40zqwfSwmXAkLjL8mR5HeGoP9DSkgcfYhlb4sK7-97tSBlMcZhYd9KEMRkQqK_N1BHS6lMGO0eikQKAyjVaQvld_05HXsIE5R0813DC8PhFZK1GxFMh96h_nY8c3Bl_IXs1DraSgo2EPF5sx7HnY6alpk_3_1frHmTIaVSuHdDKPkQ2_5pkkdCV_nQgjU9tKhFYIfL1fETZL21uNtlKLSE1UBQlbw6b5LSpy5tROI5Kfq-0Da6ynh_Aqvmbdxi-oCVaf2T1SW_G6DFjUWU0xDXSa2PbKTwIxFUJlVebyoF2zE1M",
	      "eventCategory": "Management",
	      "eventID": "8d006dac-fa19-4599-a336-d3a230b535f6",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:20Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "1cfed956-fcee-4f9c-bb7d-b1d512e97044",
	      "requestParameters": {
	         "instanceId": "i-ny0ek1fbv2k4irgb"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: TVZFjm-mt3TE7psRWv4wzimRYmROaE6RK6a-blk1M1QXc5J1ZqaOWP4_UilTumdJ8Uni_NqKwfRUhKpwB4zcMHYAZmYsDx9D0jaMwKBsbWQPmSLn7nh3MVpsN-pmsT4cp2LC3lUc_ql7wqWDeipnbHH2UCZxBhlun8Otv4vpF5YrkraD-M9_AROMNwYMfMbe4mfamHx7kk1Qa2rjEqGuyALHTp726hJAMv00n3Wng4K1eUJLgGITGVh592lKycF8NUD5Sty5-ELzaql25MKFIcYypw91I3rI1_uhf7KGbtGPl5mXu_ukfa7gAUZjaFmJT0AfpCjVgjsji5oM0QWqqqJvbBdTwz48kAc86JSKl-A2w--D0xaEhqRe23mGGvdPemXB4PHggmhaueeVEPL5bV74aDc9fHQhGG2NiCOa3QZPR3QPg69ddwFVyThf3tjLIoZ_e4T7OWlGBZjU8BkQ5rPdwPbrvwpsNJjcUzP7OLaxnviUFUhRSBwhZqiI035mI1kqtE0vxzbXNwS9j5RIfjv92BrvSFwNMZb8agK1Q3siL3wadOqNGYOkgyLkVk40kRdy",
	      "eventCategory": "Management",
	      "eventID": "b1f34826-4e8b-4527-b17a-ef9cb24ac379",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:20Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "195c9d73-fd82-4ec4-a72c-2ead0602b322",
	      "requestParameters": {
	         "instanceId": "i-p9e9ocan02xrzude"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: 4DDnYxe39i3VZP2qCvPfBcHUBBBMcdYYSyzhljgHjyGL6996txALAExpdhvWyVLfDOat8FRPllNzoixRpTCZWRlo35Dg_FnqfL1IF29WP49Wy1973IXWcqE4uXpt_F3IF8GsCnoKQns0KAyo9fLObSFnt67AwSxAgzsi6McdREq8cIg0mdIjCK2nhBc6v1VKCHuLau_QUzLh5qI5BgRDHK6FSggymuCyI3uUsNnwRfR6VT4RCN5EoT5-_aedTBlLwe81MCo3azLKWwsv6JtQpL5jfxoy-4Txygq7KNPMLxX7_HHkLPYhWy5x4CKZK-ZXqu9biSwcUJrkNIpCqUmgLV1rDtKoaePONy5Xo-TunhCkN8s796aU3ij815Hsv0OVXk62NWdg_pcnnIfon-YWM5empS0xLUqyBeHEawYAKPO3grDGlMxVfovIV-uFpmR9KdOsW3D5HAkq4FNi_2DGF6IYSY-VRxYxv40P9TBovXH7BTAniJNA1A6ilwzseqiBdtKmHc_2EoOkBTrQtIufDmd9PyE0aP2vCfVOz0pemh2ZPshJjf_8l5tHYwGBlJgpumo",
	      "eventCategory": "Management",
	      "eventID": "eec493d8-1181-4ed7-9d29-1de1e87ee98b",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:20Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "c029fc7d-b85b-42ff-8351-31aaf6c1225e",
	      "requestParameters": {
	         "instanceId": "i-yjhbbydwe3p29swd"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: JJqi-rC3zfBkVioszXW11DKpcL755AUVY2OJmrbbbxxXyAa3BGd_pEfBQfxB7eAHuDH7CPVmOf4EG1MkQKk06tnOefWSBDhlNi3BYpuA8-6jWQsKOhwShJKF6ZNVSQ6ivlccg3o7A5IShFiKJVQYGTQZ1Rc-PA8hPANFEsT5Gl2Ag1jPol68k8oO_8E4_cHKqQjvZTZJEoMF2tZwAXfrjU-EX2IY-Y9l-ONimiyuPnxchC8HSYViBz4POEKN0gZhid89D3IWLo2k70BQDl6j2L2zIr6yMVsj2v-Wc8saEaiExv7QK4NkT1l2MEEDKANkwVWarRlYlI3ku7f1H8yTqMXf9WPcZ7DfcPXoR9ich6AFDVD8J39S6kgSc9P6cq_V2yssXqcSJxwQqBkbUrPRDMlpj0VgA9qU-Sx81uWiQQTJeK4X9wYHi2RfV6AkHCeIOi5viQVR4xNGVird74cvtcBu1SzMccOkyD0HCBZ9CcnyQ7BohNuzNC17wm0AekIdxH0pZAM3Rb2OAdzXK9zE37qc-Z2F8tGPGsCNJVwP2LSQetbu6tfhJBcQpfFi3WD9Wq8",
	      "eventCategory": "Management",
	      "eventID": "45b5d41d-4732-49d2-aa3b-8b87a1c4d8e1",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:19Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "efb7d185-ec2d-431e-b845-52b0ec9f4bc4",
	      "requestParameters": {
	         "instanceId": "i-xjos2kzunblws25p"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: Fy68QK4IDJ5WWq9w6ufOr8E_KHl6yFBFh7qzkE1p4XKkMGsrvTtGPoFbKsz07ZU4sEXIqlr1_TYeFkwdclvyYKs4beqAEnihMn5cbQHdDT6peeTZvDoRvdlJ4K4MAJFsNujyWcC5DMyiCOBwWnn-I2iFxQuRcu6GovxT-uaFg4Sf25imlhuFrUxZzBxBP17gEwNx-64eP-_67QBBcrkJfxs54PTZSqkbAFB-jbJ0UqRE2wCYuVHRvWKlOX6amkuxdKOcGlHx3XJku8BccJZNBkGNBTIvkc3lMysOCeB5HfJDwfIUIuLwCk1hB3tm8NiWmtNnY6NcSGDZi1htncI4dzNGZHfPEHhJBXBzUCJcCfpeKPUNB4MATcztCL_jwfqP24GTqjNsbPsusrVOoBjYoCglljWwr8k2ltTj-bDR-tbLjRm-wkTF_25Gg8v_FvHEvE9inR43IEPtRdw6ULlwVIE-qLaYXhqPJmPrBQyhVCQLsUcIsMlqd6v9NVjIJxXRvmR5KcLJctOTykYZXOwF7Vl4fGNJT9eR11nzkVTxfTZaPwv--34eB6rZoJqJEG4IbyDJ",
	      "eventCategory": "Management",
	      "eventID": "c07943fa-79ed-4f9a-9bde-b0eefcece09a",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:19Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "8408971c-2a61-40da-8455-3b5cb32e3b6d",
	      "requestParameters": {
	         "instanceId": "i-gjzajayb7tgntj7f"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: HU9P8R2PArR2Z01od-GTeTwJ9fw_N8JCXcNkhA6psJfqSID4sa98rv3UapLRUBuHqmpY_xlLKyLSAx53FDmHmFpcVxr8_7U9ZF0cpa4BNP4o90TZx1aI0rRYJU_zZ0NapeIHGfdZwFFnCV00oJk962hfwW-ufpsJ6ZNBczV-5UD_8yyMUlPA4R5K7v9Wz21OZxLZwrKEgdj5XXdHpbpojqpCl_dgEyhGa8Jddoz8dj1cZcuAmv8BNizrUE3ro7A6wU2NSxVT0o8J105EVaWz6IXuucVfDHhK4uApI7OSTMmJkT6D5K1Vxnbgk57-Qk7HOPOBbIXQhqt7Rc4-d37Bour4o71o72KFl2KYKNdQP2qWtK9uAHk8zaxW2vhjwtG4P9mLH_UEkjmZgVlqTxbyCrY7ErAxJ0Qv37oYOQ0sZO_02fY9haXSXMedpzIbw_EUdSsxw9bPRSQcoeplA6CidjS366eiouQJOOB-iHhut2_70izsKLl0-uSpJO-MKWE9mwYGgVphX9UlhpBUVTrcWBUv3Rx8HE7IfO53Pki4WIsEtKS8wVJ25erdcnWSYMenJj4",
	      "eventCategory": "Management",
	      "eventID": "cf7fcd24-2c91-4836-b460-d01f837d5db4",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:19Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "8c5edf57-8692-4d9c-95b2-fae37791fd31",
	      "requestParameters": {
	         "instanceId": "i-awmjjnq5sr691kgp"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: glknbRaK-8bXUVYKsSXr-q5ysgD55hn_KTwTFbjiPl-hGg2ErcgTWmFFDaGHOT23Qbn2I2Cwz07cgPqRLkJsh1mM3TAlZ1yIdjjeuv9cT1eX6tMqem1qrm8qRbWxi97j9KBGu2yHsXm7yHi19qM_ddWyutsm-NXqG2e13FsP8KxPrtQkxXQi4bvZ30HHpv4hqS6-06bUEbTJFbU9-PBuCowkQDXJs7EPuR5YhlXBWqoahCNXc6V_bOKz6rR1sJOD0nZvbIqPompZur2cyAItV0kfQl4SH6rzvkk2T2jVnDz5NU-xnvUJzN3nnsc3LXjOUsBfHu4_JQPfonyRqewfQ06vhnU3gzS_0TkT_VbEq-1PBmtTRXFGEQ9nPDMQuserPuhSn8P8o5dj9uwBaLR-hZPqN64-R1mUyWuQUh3RtkwI5MqEQFu-KSmZn3TDovoqZu9uayFJaMUzdyzVqpAyB5eg9ycClfZFgYchEACGkISXj1k5iyWUWr8lnVrPhXv5I3ERGvOP4gQl2VQS0SZx30DT5ReGWKxWwsElmxJeeyu7ZjsN0W-bNPJ9gBf23hRTrzM6",
	      "eventCategory": "Management",
	      "eventID": "ec79b60b-6bc0-4a75-bf79-45a42db477df",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:19Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "9ab8de47-15e2-4e13-9a14-8eab5c92b916",
	      "requestParameters": {
	         "instanceId": "i-nzv1jjfn03nnujti"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: gEXNHHFUYw8y_MNHkGP-98XPXSVvkMEu28ZFGcP89GKZ1im05s9P4sCpRKtajVvAJzfILfA2xEmN6aR06VV6qdmstPr95kfAhvHsY0yeIjJHz1pXj_ZNO0Q9SiV-ZaAcjH9LK5Pl8muiUU2j5onTFYWbDW1IqS-myHOBQFcs3jUEvCxbdnSHwxmeVLSrHkZEbg8cWkelKkcyJokNcad7MWVbmfJNeHLaizgZfyF69MLAnHTAlC0VaxNd8m7UbkZYydMATTAMNdrvUxRhZ0LOq8yecg47kGfUUM8K-uZk0qzunzC9IZ1EGHHAQjtI9VEf9HskSA6ibh8j4BhfBguxnf6USGHIq7R9Igt5bmZ1fq-COIzGblYOecicHfilaPeEevmzbT7vcW-3dgRPK-zr04-H_0o7wyGU34mZlmfV823uG4oM0nB4JuPNd7Shflry7deP_3nvj-Aqy73d7GPicewhRVEKYDeFao0c5EevJemsepKqc6GDe-Tc6GKL5UBG8payl624Eq4NGHZa4lKuMC4t1Y3dHs1bsxu5QU2jLeVXArdLBstATsRblT-CXKDw_Is",
	      "eventCategory": "Management",
	      "eventID": "f2d94374-18b0-4479-8585-d24f7a58e3de",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:19Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "fec853a7-4df0-4410-8d1b-d86e0cf20bd8",
	      "requestParameters": {
	         "instanceId": "i-gz7w6xbdutwhlvb2"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: nKaaIO7OhRx8_gh9WeWoXY6JtQq4rPq82-RZz4uYdRG1pdkeJx75OQ_4cv9JyQYlF4vgjg1TeP6vSXcI14XZYu1DA0hgqnYqyqKFFPCQglgRqfKLTphNoCprin_-yalFcBYAhOyfy7thU8TNTKX26Eg1D9JRE8kpcomB9ov9PUQS1v_doljaouQaQXBrlh8YD5cWbHXlkf0Ahi1axtD4qCsz9stzfYLtxwr-KWXKPgwQA36-8j-vzgFUAFCvDMSOS_7IRUh662UyfPDRnuJeigPHeHdNSvdr9F9TH-Cht9GaFF_kFBKWkr-RkL0DYAOFKw2_T1g24bk_j7JYINyHIhS5MDihvlmKaAHH0Yoz_nrOI4gbdL60CH9Bhw8E-7t7cI7_Jqplqey3rTvzxMNVdpxtk3aku0as4ZAEM_LPElxfs8ZZmfY3-NuanGt0MFcPYxDmbaNFRhOk3-m0esaVTf8OsHCbeXE2erqZUWrgh3-96jx6t9hSQwdRsaqvzImXiX87EjO0-zKxmZlT98xRprqw_Lr-hdC3IEVh6wY8YFYjFOh5I4RcTO-bRkxZgH1Qfvw",
	      "eventCategory": "Management",
	      "eventID": "396d8a62-46e7-472f-b046-5c41a75ae61b",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:18Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "d4fb9e08-37ef-4cc0-9d01-0dc7c694e554",
	      "requestParameters": {
	         "instanceId": "i-bf83vbyeoo24svtd"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: nH73aM11PlKi-yyEWJlllJTikqqhwda0HamvlmHPY53dt3gaTJVbwGB1zVfdkb7oqY9N_9d-v9oqHixcCcMcOYBwBQBnJ-rVW4FxsjBI0pPSVYoTYOagpkUT7ceRLKyXWDgR70ylwVOyaKu7AJsCvSy_A2_bi2W8BirGWL3H7-Nyeu3LaKK9lL6olrz6qla9_veiB75Cc516dE-gsAKNm4jd_N1pC-WCMApGlCIYsqrv0j2gSKjP2SNlDaINPL35dcSA8syYNt36SwsgYVo3DUPCrad2W1fQ4R8Wim_GPLJwPYueFvttYNWEiPBj7sd_Zb5yLvPKRCtrxu-eYbYue1BWthbbxVoKfecgieELohPNj0MtdEjKY1kAyMnrho2QyOjdGpuX4C4gTeCytuDrunH5bDRKRtlAGPhRCsIfGFsrq-fTS_FhgDXjMc04NcJr4AZ9j6yGf4u6vMosWFi6Wg70n-W0AluNUBNHVcnXO4mvG09tBNLOmx66LwCs90A5_G2ll6_Py2vP3pXoVXUdG4rpJJhMwmVH7FYE2fA0fgV7Gr-f_yjzL-CiMiB3UNWlv2oX",
	      "eventCategory": "Management",
	      "eventID": "71ce0541-113b-4b74-bbc7-5ef364318787",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:18Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "12c55ab8-8de3-4d11-9cb5-771de13610b0",
	      "requestParameters": {
	         "instanceId": "i-qsdkik5t0ihwxj43"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: 1doSO4EeN8VCyyAelPL_ne9oDrtREHT9ciU3ZTtSs1As1v3mEHCVpeUarJxr13AWmsoIt2_yTzT1NE4Ur1yK9S0V-B6omwpgEEnGk2ZPzhrkCqSRA1flcMwIKXKchWoDB4--TAgAfHyUem-MO9IRc4RIJniE-BNY-kK_GOR5BR7y9yTy83SMANMBHFgY_zDY3Qlco5B0jmuXRnhSJXslqpL7KlXdxTLK-j1gOFIrWpZll3E8WQdCw3Sth3Btvxgj98rNDa2vfqGOxIacu5PDLDvvDTD9Dad5ceUN5g5sYwbTZKX4nbRm7UC9kp_hN_heYILrJR68VF2HTGqOl04-T-aygq12V-WB82BR_oXAuZyOrTHoUw8H42WSiYb_VP_Se3xoS6QEGsK165umOB8-ruZXG0J9M4EZgptI7b1krm2VbO5wur3JTjY6m4kiNT0baMvI_2CGhP5hduu06rllFf4Q0hAqqvHC1wsqoEUe0A36xOj9RcKL_rxQ_XR8gnLd_l2-9OCmGk3usYbhZeb1jJboZclzyYXoCCfx-nJvGlICE9OP_sutVFynLyT9QG_-dv8",
	      "eventCategory": "Management",
	      "eventID": "8aa69c7f-117d-4010-b7ea-009cd1f4f5de",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:18Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "9f89f20e-ca42-4d5b-afec-fa2da8f55fd3",
	      "requestParameters": {
	         "instanceId": "i-jcoba14jc619sc9k"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: RKjtBacdHpynSBYD2rFJAJyGRi896ep_wzschdrXXGuhTWwH1op5v_VJ1oUbV33AF8uOXxrkx7rjRJIBPku6lhMASNBInXuS-tBXw9GRd3fB4Yh6u0kxQZP95-RRCNoRGc21BTmVEegMgNPhXMG7gxA1HUJVcjVAwAbMUzBv1VEvYhHPsOm-SDbCR_vlJbJC3dtDLetZuxLoTTrcKhMMU3pazWx_MCTEV5Fn13SJMV13Hmoi_x2JrCUfAVZdO4bDePX_kyk2H9XuBmiQAg-h5Ba3HvkUQP-wBNC9cQ_Ji37Vx8oBQO2SxdqXiLHbx4W3AaI4ag5iDuOURa12a_xoUAUrP7RB2iKgr59mpC6IK8JUtDwRlv5jKYwfQMC3TtvvDtTmL3Ljxoz07_fgCECADIANklTbTKnfByZZ8XWzURr5mGxHAQC2GrDHaoJpt84x-k-9-AGNEVbOFycJJsDOfUSTQQvKIBq2CIos8bKwnZJQCVOYCwgHDqmhXyS8KaQw4OWQleQKMvfp8aZ3Q9gFxlSJbo00UqiAIHWVOUl5xhL0reKKGrL5ve6mBnQAVPY93get",
	      "eventCategory": "Management",
	      "eventID": "9fa0f6c4-dafd-46e6-af33-264c70b79add",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:18Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "894f1727-ca4f-4376-8313-51b8e5632526",
	      "requestParameters": {
	         "instanceId": "i-yqe6th46jb26scec"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: PYyJbyooe4ak6qMgask39P0gOiQ5cBbfHnbhq03IU21H-MyHGDAfqvf1w1AR8zTvfPeYrt-zWX8A_TTHbJHBMOEBBMVtdxHIVHnIPbOsU36JnpqjT4T1uarOliX6ViEkBvKm9wtPKFj4XK6xv49tdy8WomHqDsukCmOldH5KOIBDFDdLZvvsPotW_GA-HKR-FjVoRi7l7HCHDad5M8ruK1g8a8nUBEIKqbOexvpZiyJF9yO0I05X7nR81yYvKDAN4Y0n_VKUlMyS8nLYTWJh5RCzweie8uT3unJDHS24dvk51sEkrmQvh3Kpw5EADofCBWiTabx6zdoPFd81WpfOayEli1n2FI5zzeROdvIbiNlvyKjVTmcgsXYphfjbgOLeSU6bMF68_SPURL1Ua23ZkwkebQRav40J4rrnFgVWHuZbvAeULyWDEDDx_10jB7leB9Z6yAVlBqL8RNb-xsAKnk5dmvqsCsT5P53m9kC_g4389oV0LUahYu9c9fIkrj_3DJ3mZztALQl7l6fIkT_npQfg-QqfZx--t2sQW1gfIKQPXkxmsdQhdvXWik74wd6t_N4",
	      "eventCategory": "Management",
	      "eventID": "b8a37387-6dd5-49a1-b55a-a491a0bb85b0",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:18Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "8fc4e269-7b8e-4123-92a4-0821283c590f",
	      "requestParameters": {
	         "instanceId": "i-qivk0oox9ac6grv7"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: FvDsnEXWiuIoaTAltof47EUVg-dVIlwI4emrpGlLM9ElSpuAjv-7LPppbkJa9spadx-PCqvteb8TjhsI6AiunSA0tCPufgOiRyIioV_HMK1Bpj5ieQYhUIBJ-xUJx3BlwDu3aGPWRyBJNe0J3aqqaPFm5uIA6OmeQol_Qi_LCbYkcJUbGuWqxg85kE4cP42Ev9_dZW3xvUQgbvEKZGVbeVxQJQTIDChBXifHRxOtUaykG196i6lg6xR396OSGs4mfq-bdxNKYAKssZaOvPOqqf-43f260zDUmI5OohcgrPSfNBrGIeXzMUChBd2fNzIXA8-8InOL1OqD55FB_cDL2rhx3hqdCB1tOhxjUNfZTAAsfOeD3QurNUew8oEUP2LE4x74vtWeSR5JiZMWGFPWxoX9cycXnJ9enLY5JePWDEmkF0toZ0aFzAYha08QhpXD1YEVWu9C8ZkW4aa998ZX6C2nP7GInZtN8CBM4BlSi5NAHYpZGUl_PH7YWlLGq54JOMh-JbQ_FiGms16beBvJqsJyS5CGvYoEEnjtTEYDrqxULD0UhxUN8LsJmYOZxw9FrOI",
	      "eventCategory": "Management",
	      "eventID": "c1e66f32-ddf1-4e85-9a5c-9b11b09e2d06",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:18Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "98a10bb3-07db-4576-9edf-73d8d2e37460",
	      "requestParameters": {
	         "instanceId": "i-n2d16wuklpqfdsr9"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: aTrFQVq-TlNHs1qYAG80Y-kgjzT_ie6zqlFDxIfbXqvyqCsVEmFK6CieWIHOBEhcMEsDfmEpudkmch0OKIeZHgCYKrzIzp1aoHfAUFeUvUaAbq4SZLlLjeCOpOrFgCLROeGzk2w55VAxsC0JdhAtI-IoWOsE3CjBDJ2oJO8KpFy1nLVpUA1VU_sJ0cJudc2a381zduNnnKJufvt_xr19glMtN__HERdIWJguV9NinCtviEFOa4-Ipzj7Qd6zuQ_rYAEmM9jkAuEdOfl-1fBJ1rouciEwao3Rvpz8mMV3bkzVEb8pTKIn5X5vp57v7Xapb8ZP08UpGeswPz1u5ybB__EgmHcW8JS0Y_iWybVslZTruLarO5JbkIlv9hE7viVbyfvXmnbrnlRQHYuyS3Rt6aYmvdwqqMjd918qvpI1rWeILu2URb5M4dK1vNA-9AxvAUMZSGViaJxncd0rcnDPNNUaSQX8bjetu15TeLS1G0N4fdqD-lcY0Dc_NgjNwYTcg8uXXXLLUKgJ1lKpkeEeSXNImo2X_DYTwCj9xkLPZ2qlckqNeLokUqdWl6sDZpHAyPY",
	      "eventCategory": "Management",
	      "eventID": "1078f3f3-e72f-42bd-a0c8-7f321b5fce0b",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:17Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "0bac5486-48ac-4ed5-b3a9-c094ee3a7304",
	      "requestParameters": {
	         "instanceId": "i-bykprumj5lnfe4oh"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: IgBTWD-QgF4jCm2kuMMIXUGemfMfC7Hd7-UTvXmtYd4amt7MbUaC1FT4ne5XMwGaOq59YgFlane0ICbGs5Fy_zp37XvFqEVbrlu16lxiqVhgghuL6bH2jfBuuqWOGrfFNDbgXSNhZNHhN8pQ4Zhg_bHJi1jcx2XYlnN-BKy2_5vRT68-6xVl-D7MpyCh-J4PeuiyIJDwSWgT3UHzfMapPfMVRUetYSgGeub_sxMswfiR1dxD3PaUgubNNzjiTIeoElqxdELcDE_1V0RC2hKxuq1-kj5hXl4_hEzmuicGynwhkpXpP8W6u8xq-S2v-of5N5uBeTafwaDAtGIFprBp8smR6X3OeyB72nZVeyyaeIlL3uD2WkhX0da21OOGYRDTwbRBazStsugyvY4MnJWu5PCk0q6XHptm6qyL8nuUfZUkp-NQp35CKx9HaBsuLdvFe8dpGIwy5DlUes3T4IqITcZa2tA45xfeGAqo93G0LRZgQ3PMaJvTqW5hgN_6XXvt1_P3B9S6SCVMyR7Gu5mdG6fjbDKtIbWfeFz17Wd0fDSHfoaT1plivwSgZrkgnioCFQ",
	      "eventCategory": "Management",
	      "eventID": "735eae92-16e3-469c-b454-4507c47aadcb",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:17Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "b195f9db-6777-41a9-8797-2df84ebb07dc",
	      "requestParameters": {
	         "instanceId": "i-dcw41yq2wp8h1d58"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: 3Whk6y4yZdhA0PAEz88EFn9PqfvCo1S8surcpaXE6332jpdvRht6VTm3WWdOCkQ2mUq-zXlY1GimOJW6TJC9SkPtlBUAH5KxOFAQPMymWzNgU606sUYH41P7t63dp_F9_pVVO3gj22FW0qv1ZKHIIayypQ33bHS8lT3FQgqZzy6mntCT6OVSYJ5KiZEMmPiMLv8nVcGPoKHQErgjMcXWtkSuuI4tq2xhQBjdJlWgHDNv1Wn0M1RYy7_WKYkgCsoGlWSb10XMexgwl1dpmhODFZMA-hBbQZC_S9tKE3sTsuIppvqIW_SFY9WLdeI0_GRtjBt9hHNKBFr_V4GmNFapSDSMjt-w_OeWAC4MqmeGR_adqtMSIiamRHXtHfoEK-0M3c_HCIAl14XBPg4pKnCZiCutGk6ak0AVJmjz7iBWtkduRfBy1yk_7iXypjmLkUC2dCGPe3NYIm-hYMrlbqpFnZmyQf54by9MLj_I2h2Rjf0RXoRhFnwURyHtO_D9-jsWNfO-qgq0VKCg1gqFv5NUYfUQKb8CNALzxCCEjQxrgT-nkftGRxBNpLSs7CEwkyqcPg",
	      "eventCategory": "Management",
	      "eventID": "73f28e1e-7fe0-40d2-94a4-cf42930e8b0a",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:17Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "de6fa24d-5054-4525-abe3-a210b4993b1a",
	      "requestParameters": {
	         "instanceId": "i-okja04dckx6yg2uq"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: XsZyApBJAXhkSm43yz8Osvyv013N5Y2d1rnNlbOkkajw43v_w0IpDACr9S1GlpC_FLYISw3CunlllRJn4Q5GZJX-sS88rpWFIWTksDCKwb_a0hpbcNTqERnL18B_VOC-aOfl1QyYqmYDcGKISJl8jp5_uUMV5A-IFYEMGskUfbxpQE1rtIWCrXGPPnhWQn9gHA5eBhZo63LTdhMHKJenjj592AhJ__LaXaxeg-iW5p9V96uP9nTGiVx529QZlVPNWVmL0w6E5Ub2r7IKYQkE3SXYa6bs6IhquB4MAt8JMnO0YaPRnEUxVOdBPa4isE0Bgl1C5-8NQZ3uSPQiu9o-udWYVKbx0xk-jlLz4xXbAUsCZnGAsgFf7WOPg2icEvol6a5a-cAx3OQd_-BAI6rD4OdquHxo5ddPIzGsB8rDfGfrh7h4-JiAxTWVJ7ZlFC7sHcu57SSceE05R7ez9x9weIbeqmVz5TFLYnA6i4jyI0cRAaZYZ4PWG3A_dH6K7caomOrHVcayeV1H88kfma5DprPaMyo-hIAewgXrmSQsIou95sA3P8WLBtUXI4rqUC6vevg",
	      "eventCategory": "Management",
	      "eventID": "a8d8ee73-2a80-4d03-ae6c-42e2964a5e43",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:17Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "a14a2932-ca5b-4aaf-89fc-c4d66708fc61",
	      "requestParameters": {
	         "instanceId": "i-8hy3natzpp4ef7ri"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: SkMlzz8Ec9AftDBkf302YkfKSCGS5zriIUZMQj4UAaXyX5B74Fg1f2f_IgZ6EdUNcmVr9A9OzxE9WmNikuJyWNRCX5Mjy_HRBg6VrxjWuSoUPBll0nWbIww-1NehYMVHla3eLDBA2KUsuE0KJ0ZAa2Cmy1LsT6kmbQ3PHK0a2INProm2fWi_k33oJXOTapMy5V4eVKIIWsCxWrFHO7o1E72cORK789yeKavJsP86tYGHdzssYRpnNYK-4y_YEphKj3Kc5NeOs2thecEMXiLPyPXJYzlG3hzDmd3vU-sgbC7t3uCPMuw0mdRWvd9QaNKp57dAP1Bl3CH6CEo2iGuftLyCA32dzTpAG1khB_2ct9Yodq28M7j4Cp5hC0q-IDpUol4hUjeoxN7QLFzrn6IpFuvP18PlJY2VyrMS05Mc9-Pv0HW6cen1p3ooH0qHAlvsG5LO1aNX0xacTlHAthoIjziAAXKD2AQBVtbo4rh1ds67tcLvaGZGwhv_uyziy-UYeBU_ENloIGFMmD44m4leqoXQaessC56tbFWmJEseRQtHxuA0rslcPW0l2Y4EHQ0Hdg",
	      "eventCategory": "Management",
	      "eventID": "afcc764e-2db4-4fe2-aa74-85d01843d7ca",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:17Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "36fd0bf3-e2b7-43af-bec6-dd9df405c462",
	      "requestParameters": {
	         "instanceId": "i-ymn0oq6iadzm0v0t"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: l58oQoHuiZkxwnQ_NKmt61fv2TTDkDEIIZRKFdxXk1cbyA_Mz38ZetF794KYJVPv9zh-UC3ZtvX1WJJnAKIZXfjA9Cy1i8lSj7zHv03E2MQ6w6I16hErXuvfbNCOIGWskZ2_H_-p16hqtPGz38n9ZU9BTXPUScqUcA9u2vi4aHfOyqBJTl85vPXl0PNX0rSCNea01NDzwQrdxme2UyAiuFEa4CZceqFpahDKOA5S3tZm2OzBJaZdeBYTgUwlcJYmM6iEXiC6ZGJsi3IV-rcg3WGMFogLXp_tQTlfMcjiPqO9v-LGyypMT3aVCWfzVTnJrDk-7-S7ue7zuTlN8y9LHWTaQvZFf6vAMEe5o8DG-W5cEBoQu5BgdC2yLJk99q1wNM1hCM6xSx9MI1m3Z4FYQkfTg6okRUBJiXClAlWVDXyS8r6KqAog1bNB3XorSP5TE9FgEq7stZ0DUzNvYqHtkkEEkfS1PWmsxPFBm_ew1NPvAptqzn7dci-Xo1XcqWFxqDcQEdblBhZGweU9OodEznDv-CkI-iO54_Zn-fMR0WP58pSgNxiP2x00PR2lbC2WoOly",
	      "eventCategory": "Management",
	      "eventID": "b5da67b9-87fa-4151-bd9a-818f3237fb91",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:17Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "a7cf65c0-a900-418b-99ab-a5d2ec35eed5",
	      "requestParameters": {
	         "instanceId": "i-l2yrvbrcwc3ytkcz"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: ZUocdTACiGX76lWaShcYz3NiFH7xrcQLJF4xvkdVwj1YRi1LCjXrOGw99KaEjQryXTvxQl8mcoL3NPIH5bvnNm56e4dz1U4_VU1Kxe9GM2GOFvI8Dtz4yeL51wDiwFmt0g9Bfy90J8IevWimq6H-qiLNMbvL8s19Yxe-IPC4EOExJ73IGCm2M0L5Kk1PI1FNzS0V7JnRS53ZBBovZxoY3iJ5KDZ0IJMTbemzqT4uu4YCPzcsnHolRL8LaKniskKGZ4XjVxD3b5pybZ26C7DE77Wq67rlhNwJyRM8RG12tety1tw20hwblshCbJUw2YoR-_UffA4ZbMMDMSS1OkxatoynUOee5zTrapuKfsI592sH5SNLDH2nKzTMu75snXpwMEkkarPJR1rya1g7BQjvB7LcE8lnQV5zwXjCuwLx-yZrDNW6sytsvLt8oS1ASdIJlZk92V1rYCRvBBbMFgIA-0eVACBwrBfrm3x4AGM2YWBbtqtsZUYLr5Ofr6gJWn8xd9Ve-KZ98feGVI0hGzX2RMFbEDF5CeaztSYJ9VnOrxrVH1Cc3oE0GbRcBikla_4vq8_u",
	      "eventCategory": "Management",
	      "eventID": "52242c8d-7ef7-4165-90cd-621ebe835388",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:16Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "0436a948-0d62-49b2-a53a-07f590224fbc",
	      "requestParameters": {
	         "instanceId": "i-gspajwz8z9wrutsz"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: X7IcIBBP37fUlSTM_9cAnZKJ-zTlv5zmnaUcLS5lQZQfMyq3jfoXbih6NSCKKrWRnPqrCRmxo0uAw7ZIx0iLur5x7fvQq5hq9-ykkM9of1GB6aycacQC7yDzZmnFm8EHAoI3prAsEtL2e6DXtfNjT-XT0V8n69-2o8DVmh5gT7J4MZbfZssfRF-kdyCH4V_QVSv9Greh1Gnluz0EmztA6YAMhPCYG9cXp7GFzeQmQswsocXIXIhziu_UrwFb8hWZRM8Ih4ES3pvcZwzC6UB_bvSMjsVIjrJpNKNhmSievgN-MZno6buBDdsVz7pRCJJzFzvhsdj5S2e-I3jfTTfucNpyZB_xpyuSCghSW63oYi3mL8ek-t5h-sx23hANg523FIRk9w9YI6mmHiK74cwO-OUHgFNd8KERtSXHUBeno95Tp4ONhO6wSXYE6pJj3IevrcmgoWu8IHni6RbNeTC8h5SWb3sknXmdQzeN7UwEpoEEPhWtegFPcX0Zo0vOTb0oawDx16Y6eryN3966VgE_6nuDuCPMSESJngEnXZgtxLDDx4_lVymADHCS1G2vdh7ATuk",
	      "eventCategory": "Management",
	      "eventID": "a9da1fbd-464c-4b74-8c64-96eea2564978",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:16Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "8fdbcf53-b574-44a8-91c5-b81f183c871f",
	      "requestParameters": {
	         "instanceId": "i-ce85lye2frdpml4s"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: 6Vmm3_Z2lje988np7WwvCzM0gOYUowRA85YdDAIV3rx8y1O2mvqd1bWvJ0Uil_jPCbaRHVGEwnKbxuOEgMThvNpKooEdt2KRMbgEhUvfsdBb_l-tT5d5HM2wGr4t9C5u6uSIj6aJPYtvNrLSYZRz5oAFnjuoJb2m_T-63qxhnVpYvPswmWAUBRhHN7bZs2UVAGUF51CZi0bIB007D6MEkK7vijtzB54oBEZhedPhsLG4axf570Oh7fHoXBKy6AU_W1n-giLzqonpoUsqVuV5K7yTdpJpt0CKTPRYpkJ4ExOF359Q73q0aTd2aDnlWgryBSDVQQdJXHz8zoBOtVF3bl46JK0MTriGclPhz4e-k48Bv9gTMLsyasPIYbf5OwgkKgSrWa4e48F3QRfi4jMe_P9NDIKYQG-vFTyu0hrVoZWbY5OonzJTqYpgkmI1YgmZgKsKIFuKbO37QtAbLPQFJln1vc8cbRbKo3yrIuhiJ0C-lmdr-9saiOkGbcX-iPETeVh7LA8RxbQi74v7AVKq4y8T73bvP3sgiOxaHGx_KD96E-lY_SBy5vvP7EDNUCJO4zHw",
	      "eventCategory": "Management",
	      "eventID": "d2d44fa9-f50b-4877-8a52-9e3855029970",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:16Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "2e320d8a-8922-4741-aefd-86cc33c99f2b",
	      "requestParameters": {
	         "instanceId": "i-z4rfvoc4sgtoirf6"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "Client.UnauthorizedOperation",
	      "errorMessage": "You are not authorized to perform this operation. User: arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000 is not authorized to perform: ec2:GetPasswordData on resource: arn:aws:ec2:apiso-northsouth-3r:457448411975:instance/* because no identity-based policy allows the ec2:GetPasswordData action. Encoded authorization failure message: H_1M2f8fBtX-nKWuNweECYRadnJgTd8yB-qZbnIYTBwsE58jAcA13xaXwijpN2uy4ksDhtIwclLwy4y5QxG82pYgzDWogJx94y_UP8_Sb_MTS9xBuWqjmelx0Z0QrF65xf1J79Gj67jI01QYDjVjuIPHR5_ygzq0QUzNU28lcbPiy42MY1GDPp24x-W3HVPDcnOzfTdqV0T-rKp9dVHwNB-lM_OPx3awGgOkofGAsRcP2aduNxYJcATRXhoTczjo7Lvz5rIKp3u5rC1JQDXAxnJ-8WrxidXOcVnTup5nNrkWIo6ACaoupxIf86yS1nJ6drtfU-r2gUuBhduI48K0y4PHP-2AFf-U201axMzqCYZsX5hnWf8hRxa6VLKFMJVsxsuFxZUVAAwm5K2NsEkzHh9T5KWWR2vO7pxFp-BgiarX_5ajJyVeTmON9LYJI3Gqit5eCV2F1mC8Cvy-jvWC88dt_qKzSTKtb5RMwAJZ4HivEXqp6iCdlViSJXbRGK5C3odmUCzGMUs2wV6fMAAcKWinQobra0P8Nn2zzKk6Zqx-ikgMwGDLZ8C5FZiNpjVUrv0",
	      "eventCategory": "Management",
	      "eventID": "f1e17321-830c-4761-854c-158258e915b6",
	      "eventName": "GetPasswordData",
	      "eventSource": "ec2.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:16Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.09",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "faae9c16-fe9a-457e-a12f-41f71b7469f7",
	      "requestParameters": {
	         "instanceId": "i-rbn1gvh843rzs87g"
	      },
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "ec2.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "ASIANQQHHS551LWULIWD",
	         "accountId": "457448411975",
	         "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	         "principalId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000",
	         "sessionContext": {
	            "attributes": {
	               "creationDate": "2024-07-30T21:31:15Z",
	               "mfaAuthenticated": "false"
	            },
	            "sessionIssuer": {
	               "accountId": "457448411975",
	               "arn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	               "principalId": "AROAMLQ9F6KHQ07JKA0WY",
	               "type": "Role",
	               "userName": "stratus-red-team-ec2-get-password-data-role"
	            }
	         },
	         "type": "AssumedRole"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "eventCategory": "Management",
	      "eventID": "d769ddfd-2cda-4cfa-b33f-05d3b886921d",
	      "eventName": "AssumeRole",
	      "eventSource": "sts.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:15Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "556ffdc4-27d1-4ce9-8932-cdca27641708",
	      "requestParameters": {
	         "durationSeconds": 900,
	         "roleArn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	         "roleSessionName": "aws-go-sdk-1722375070115152000"
	      },
	      "resources": [
	         {
	            "ARN": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	            "accountId": "457448411975",
	            "type": "AWS::IAM::Role"
	         }
	      ],
	      "responseElements": {
	         "assumedRoleUser": {
	            "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	            "assumedRoleId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000"
	         },
	         "credentials": {
	            "accessKeyId": "ASIA7RQR64ZW9JXKWPUO",
	            "expiration": "Jul 30, 2024, 9:46:15 PM",
	            "sessionToken": "IQoJb3JpZ2luX2VjEGYaCXVzLWVhc3QtMSJGMEQCIGWKgbykBmpvw3C2tHFJG2Z2EFmm8TFxSMzImeEIU/ZRAiB0qwHtoIdbFspr1VflqA8ScX+dd4rMEEsERlrKK7oVoiqrAghPEAEaDDc1MTM1MzA0MTMxMCIM5ByNCGn4qrkq0GCVKogCwZslL1u9JTIgKxy3FLc5DbnZJDGn8rXDgSa/ePdExCGfg+0jLmpH1UB9bjdXgqf3fVywvv9n3dKO9oI53KhIDxEnt6jbv5g3l4THATYf8KSx32wBKxybA0s5DLSV59jQwT5iFtHZWYRl19ntvhGoi2TlcBfN2QUlR5NjfedcUbpV5hSWwRtU/6tX9PX18qAJxsFfg2zlvgApf93ndSh6esupNF0stzaVzroT1WwSqZDN6onFk/7aZ3bFHQePjWT4l4e17hRbshVwqRNtaZdAIkR+qctOec1bOXRyqk2Fwtm3pAoCF7zC4CmBhav8XA7s2ed7lGI6ZqNG4h44J8lgVuCtf7v9Y/OEMKO3pbUGOp4BWl8lpF5zj8RzOSvPSmeOfCrHx3IIGm6P6Qa6jhOGCWCPUMEGyq3BrXnmBaRrnhTDzErvVJw2ZB9UV+ynLeTwWTC1dK0zYV+6h1wrLFAq2HZEnn9nW2Q4mesr2YicUASap3zWz+28xjIm5+WoiEuSJyirL3UznA4MAZxc1wf/vvuC47vx3M/4X76F6+zGJeO+/VzuwwfkqVYMo7Kag9Q="
	         }
	      },
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "sts.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "AKIAWOGXN38MFN92ING5",
	         "accountId": "457448411975",
	         "arn": "arn:aws:iam::457448411975:user/christophe",
	         "principalId": "AIDAFSHDVNSWGFKZR06G",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "eventCategory": "Management",
	      "eventID": "fd179e25-9f1a-406c-8d7d-62f9d4938ef6",
	      "eventName": "AssumeRole",
	      "eventSource": "sts.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:15Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "880bf8cc-0787-4c2d-8564-3f4ce8946109",
	      "requestParameters": {
	         "durationSeconds": 900,
	         "roleArn": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	         "roleSessionName": "aws-go-sdk-1722375070115152000"
	      },
	      "resources": [
	         {
	            "ARN": "arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	            "accountId": "457448411975",
	            "type": "AWS::IAM::Role"
	         }
	      ],
	      "responseElements": {
	         "assumedRoleUser": {
	            "arn": "arn:aws:sts::457448411975:assumed-role/stratus-red-team-ec2-get-password-data-role/aws-go-sdk-1722375070115152000",
	            "assumedRoleId": "AROAMLQ9F6KHQ07JKA0WY:aws-go-sdk-1722375070115152000"
	         },
	         "credentials": {
	            "accessKeyId": "ASIANQQHHS551LWULIWD",
	            "expiration": "Jul 30, 2024, 9:46:15 PM",
	            "sessionToken": "IQoJb3JpZ2luX2VjEGYaCXVzLWVhc3QtMSJHMEUCIGm0kj47xAVKg25149QY6m0tHI8QKHcgIYPKJNYSkt32AiEA64SPa+BDSMJXiGD0qT45dx3H9Hj18oeXyl6fq7G+e4gqqwIITxABGgw3NTEzNTMwNDEzMTAiDJfCBLDTbEWuUNcbICqIAvzAOy3GNiobaWkep5/dAzk/rl6x/Lx+QNE+tUQnTU9xpJWQ6gl0uOxfQaQCingQ6Bwa7AYCIwghP0p+ijLHzj0WK9w6X1M2HgqcLIWaqarREf1xyOsPkFbNsML+1cw50lcxSCEXlQnkCDAGE1cI0wInkycEBuxGFDckceXf4whG9QzNW/jR7fDuzsN5u8GI4UsP77/oa2HISgg6wUT2byc3ni6+YruVQY//2ffKPfQyf1L9RmssxoYGb9t9iazDJjKDunKKZMvMEan4F9+acCbIUrBROgZ9Ays1D1DLjunCfRG9xd2fZ/boG6alhxNmuck39UfAxF1zyLAs3zmdWcQT0Z2croAh0TCjt6W1BjqdARs4DLOAmVNuEmRq1kvuWtdN8C0Q+ObHWUjFYQbcSNyEQOGz6pegmGbypeI9JSgxR7z6GPrSQS1yNWD9+Cs3LNl4Xr/zVmjDYDnVepIWDZ8xYofwlg78esvHzBbdKoKYt7se7feg1Kpyi0UT49BJvpUul9h1PGoQHF5zmVDA/QHqfoq5Ykv5haahEewaCSp6tvgljHXQ0xFDJv/+SIs="
	         }
	      },
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "sts.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "AKIAWOGXN38MFN92ING5",
	         "accountId": "457448411975",
	         "arn": "arn:aws:iam::457448411975:user/christophe",
	         "principalId": "AIDAFSHDVNSWGFKZR06G",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "AccessDenied",
	      "errorMessage": "User: arn:aws:iam::457448411975:user/christophe is not authorized to perform: sts:AssumeRole on resource: arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	      "eventCategory": "Management",
	      "eventID": "46558847-8b84-43de-8c96-302aa4744763",
	      "eventName": "AssumeRole",
	      "eventSource": "sts.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:12Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "bf47f64b-bcf2-441f-a1b8-9cbaa241ff11",
	      "requestParameters": null,
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "sts.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "AKIAWOGXN38MFN92ING5",
	         "accountId": "457448411975",
	         "arn": "arn:aws:iam::457448411975:user/christophe",
	         "principalId": "AIDAFSHDVNSWGFKZR06G",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   },
	   {
	      "awsRegion": "apiso-northsouth-3r",
	      "errorCode": "AccessDenied",
	      "errorMessage": "User: arn:aws:iam::457448411975:user/christophe is not authorized to perform: sts:AssumeRole on resource: arn:aws:iam::457448411975:role/stratus-red-team-ec2-get-password-data-role",
	      "eventCategory": "Management",
	      "eventID": "8a8844ff-dc95-4ef5-87d2-d86cc23fedd0",
	      "eventName": "AssumeRole",
	      "eventSource": "sts.amazonaws.com",
	      "eventTime": "2024-07-30T21:31:10Z",
	      "eventType": "AwsApiCall",
	      "eventVersion": "1.08",
	      "managementEvent": true,
	      "readOnly": true,
	      "recipientAccountId": "457448411975",
	      "requestID": "b3f190d5-4701-47ef-9fb0-76e8b7877df0",
	      "requestParameters": null,
	      "responseElements": null,
	      "sourceIPAddress": "200.249.253.51",
	      "tlsDetails": {
	         "cipherSuite": "TLS_AES_128_GCM_SHA256",
	         "clientProvidedHostHeader": "sts.apiso-northsouth-3r.amazonaws.com",
	         "tlsVersion": "TLSv1.3"
	      },
	      "userAgent": "stratus-red-team_5c59eb79-6dac-405c-a4c4-e19aec03c666",
	      "userIdentity": {
	         "accessKeyId": "AKIAWOGXN38MFN92ING5",
	         "accountId": "457448411975",
	         "arn": "arn:aws:iam::457448411975:user/christophe",
	         "principalId": "AIDAFSHDVNSWGFKZR06G",
	         "type": "IAMUser",
	         "userName": "christophe"
	      }
	   }
	]
    ```

[^1]: These logs have been gathered from a real detonation of this technique in a test environment using [Grimoire](https://github.com/DataDog/grimoire), and anonymized using [LogLicker](https://github.com/Permiso-io-tools/LogLicker).
