---
linkTitle: "Optimize AWS User Entitlements"
title: "Optimize AWS User Entitlements"
weight: "10"
aliases:
  - /en/ciem-opt-aws-user
  - /en/docs/sysdig-secure/posture/identity-and-access/users/optimize-aws-users/
description: "You can examine and remediate identity risks associated with specific IAM accounts and their permissions by using the detailed drawers. Simply click on individual rows on the Users page to open the detailed drawer for further analysis."
---

## Manage User Entitlements with Detail Drawers

The Users page organizes everything around the individual user account. 

* **Overview:** Displays the critical permissions issues detected for this user account, sorted by Permission Criticality and Unused Permission Criticality. 
* **Attached IAM Policies:** Displays the policies this user account is connected to, sorted by unused permissions and total permissions included in the policy.
* **Attached Groups:** Displays the groups this user account is connected to, sorted by unused permissions and permissions count.
* **User Details:** Displays a summary of  total granted permissions, group associations, activity, user ARN ID, and findings associated with the select user account.


![](/image/perm_user_pane2.png)

To reduce a user entitlements, click on the user name to open the detail drawer and sub-tabs. The following examples provide various remediation options.

## Understand User Permissions 

* **Total Permissions** are the total number of permissions granted to a user from all the policies the user is associated with. 
* **Unused Permissions/Permissions Unused** are the total number of unused permissions from all the user's policies. 
* **Permissions Given** are the permissions granted to a user per policy.

![](/image/user_graph.png)

Investigate the **Attached IAM Policies** tab in a user's detail drawer to see how the permission totals are divided and how to handle the surfaced risks. 

For example, this user has been granted 15521 permissions, divided between multiple policies, all of which are unused. 

<div style="max-width: 40%">
   <img src="/image/uc_user_critical_pol.png" />
 </div><br>

To remediate the permissions in this example, you might:

* Delete an unused policy: In the example above, the policy with 103 permissions given has not been used by any IAM entity. Sysdig recommends removing this policy from your AWS environment.

* Optimize the Policy Globally. 

  For more information, see [Create an Optimized User Policy](/en/docs/sysdig-secure/identity/iam-policies/#optimize-a-policy-globally).

* Create an Optimized User Policy. 

​	See [Understanding the Suggested Policy Changes](/en/docs/sysdig-secure/identity/#understanding-the-suggested-policy-changes) for more information.

## Apply Remediation Strategies

Sysdig suggests the following remediation possibilities.

### Create an Optimized User Policy

On the **Connected IAM Resources** tab, select the policy you want to optimize. Open the Details tab of the policy, select **Remediation Strategies**, and click **Optimize this IAM Policy with a subset of only used permissions**. You can then download the suggested policy, upload it to your AWS Console, and associate it with this user. This option creates a new, user-specific policy that considers all the policies with which the user is associated. 

<div style="max-width: 50%">
   <img src="/image/uc_user_critical.png" />
 </div>

You can also note the user's policy associations listed in the **Attached IAM Policies** subtab and remove those associations in AWS. 

### Delete an Inactive User

Sometimes, a user may be associated with multiple policies and groups and have a very high cumulative number of permissions granted, but Sysdig detects no user activity in the environment for over 400 days. In this case, removing the user from your cloud environment is recommended. 

<div style="max-width: 50%">
   <img src="/image/uc_user_inactive.png" />
 </div>

In the example above, this would eliminate all 15,521 permissions granted and remove this identified Critical risk. 

### Optimized for Potential Compromise

Sysdig tracks all the permissions used by a user after they are flagged as potentially compromised. By default, these permissions are excluded from policy optimizations. This careful scrutiny helps maintain the integrity of security policies by granting only legitimate permissions and reducing the risk of privilege escalation and lateral movement by threat actors. This proactive approach helps ensure that policy optimizations remain secure against malicious actions.

<div style="max-width: 60%">
   <img src="/image/optimized-policy.png" />
 </div>


## Use Case: Potentially Compromised User

Overly permissive credentials can lead to lateral movement and privilege escalation, resulting in cloud breaches. Sysdig helps prevent attacks by strengthening your identity posture by detecting anomalous or suspicious user actions and enabling real-time incident response. Sysdig offers the following findings to help correlate identity behavior with events, enabling detection and response to compromised user identities.

- **Potentially Compromised User**: Misconfigured identities and secrets, combined with certain operation patterns, often indicate a breach. Sysdig Threat Detection rules can identify suspicious account activity, such as privilege escalation and multiple account creations and flags these as **Potentially Compromised,** helping you start investigating promptly. 

  Potentially Compromised is the finding that is triggered when Sysdig detects anomalous or suspicious user actions. It indicates that you should  investigate this user.

- **Compromised User**:  You have the capability to flag a user as Confirmed Compromised and it serves as a clear signal that the incident has been thoroughly triaged and is not a false positive. You can then take appropriate actions, such as deleting access keys, or deactivating or deleting the user.

Sysdig provides recommendations for each option, guiding you through the necessary steps to address the compromise. You see these recommendations when a user is flagged as **Potentially Compromised**, and they remain the same even if you mark the user as Compromised.



### Correlate Events to Identities

The Sysdig **Events** page displays the policy rule that triggered each event along with the associated user details, and it allows filtering. For example, you could filter by the **Advanced Cloud Behavioral Analytics** policy to find if any user has been identified as **Potential Compromised**. These event, essentially, indicate that an identity breach has occurred. 

<div style="max-width: 70%">
   <img src="/image/compromised-user-event.png" />
 </div>

### Triage the Event

You can then triage the event as follows:

1. Click one of the events to open the Events Details panel.

   <div style="max-width: 70%">
      <img src="/image/event-details-compromised.png" />
    </div>

2. Click **Investigate**.

   You will directed to the **Identity Investigation** page, where you can see the cloud account, IP address, AWS region, user in question, and the user activities.

   <div style="max-width: 70%">
      <img src="/image/breached-user.png" />
    </div>

3. Click the user to:

   - **Summary**: View a summary of unused permissions, total permissions, criticality of permissions and unused permissions. The critically is determined by the excessive permission granted to the user through the roles attached to the User. The details panel also shows when the user was last active, User ARN, and account ID.

     The Summary tab also shows that if the user is **Compromised** or **Potentially Compromised**. 

     Sysdig does not mark a user as **Compromised**. You should manually check the Potentially Compromised User, triage the activities, and mark it appropriately.

   - **Respond**: Mark as **Compromised** if you think the user is compromised.

     - Mark as **Compromise Resolved** if you have already taken remediation actions.

   - **Remediation Strategies**: Provides two high level strategies to mitigate the risk.

     - **Contain Compromise**: You can address the compromise in the following ways:
       - Add Restrictive Policy for IP
       - Deactivate User
       - Delete User
       - Force Password Reset
       - Delete & Create New Access Keys
     - **Consolidate and Reduce Permissions** : Create a new custom IAM Policy for the User with a subset of only used permissions.

4. Select a strategy you prefer.

   For example, if you decide to delete the user, mouse over **Delete User** and click **View Remediation**.

   1. View the remediation instructions for both **AWS Management Console** and **AWS CLI**.

   2. Click **Open in Console** to open the AWS Management Console to perform the actions given in the remediation instructions.

      Optionally, you can perform the operations using the AWS CLI.

5. If you want to consolidate and reduce the permissions assigned to the user, click **View Remediation** button next to the **Create a new custom IAM Policy for this User with a subset of only used permissions** option.

   Sysdig offers smart policy optimization that excludes permissions used  after a user is flagged as **Potentially Compromised**.

   1. Review the IAM policies and instructions to create a new IAM policy.

   2. Click **Open in Console** to open the AWS Management Console to perform the actions given in the remediation instructions.

      <div style="max-width: 70%">
         <img src="/image/reduce-permissions.png" />
       </div>

6. Return to Sysdig Secure UI and mark the AWS IAM user as **Compromise Resolved**, as given in step 3.

   <div style="max-width: 70%">
      <img src="/image/respond-compromised.png" />
    </div>
    
### Remediation Strategies to Contain Compromise

The following are the remediations strategies to address a Compromised AWS user. See the [AWS Documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_manage.html) for the up-to-date information.

You can use either AWS Management console or the AWS CLI to perform the operations.

#### Deactivate User

##### AWS Management Console

1. Sign in to the AWS  and open the [IAM Console](https://console.aws.amazon.com/iam/). 

2. On the navigation pane, select **Users**.

3. Choose the user you want to deactivate.

4. Open the **Permissions** tab.

5. Click **Add inline policy**.

6. In the JSON tab, paste the following policy to deny all the actions:

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [ 
         {
           "Effect": "Deny",
           "Action": "*",
           "Resource": "*"
         } 
      ]
   }
   ```

7. Review the policy and click **Create policy**.

##### AWS CLI

1. Open your terminal or the command prompt.

2. Create a JSON file, for example `deny_all_policy.json`, with the following content.

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [ 
         {
           "Effect": "Deny",
           "Action": "*",
           "Resource": "*"
         } 
      ]
   }
   ```

3. Run the following command to attach the policy to the user:

   ```bash
   aws iam put-user-policy --user-name <USERNAME> --policy-name DenyAllPolicy --policy-document file://deny_all_policy.json
   ```

   Replace <USERNAME> with the actual username.

#### Delete User

##### AWS Management Console

1. Sign in to the AWS  and open the [IAM Console](https://console.aws.amazon.com/iam/). 
2. On the navigation pane, select **Users**.
3. Select the user name that you want to delete.
4. At the top of the page, choose **Delete**.
5. In the confirmation dialog box, enter the username in the text input field and confirm the deletion of the user. 
6. Click **Delete**.

##### AWS CLI

1. Delete the user password:

   ```bash
   aws iam delete-login-profile
   ```

2. Delete the user access keys:

   ```bash
   aws iam list-access-keys 
   # list the access keys
   aws iam delete-access-key
   # delete the access key
   ```

3. Delete the user signing certificate:

   ```bash
   aws iam list-signing-certificates 
   # list the user's signing certificates 
   aws iam delete-signing-certificate
   # delete signing certificate
   ```

   Note that when you delete a security credential, it cannot be retrieved.

4. Delete the user's SSH public key:

   ```bash
   aws iam list-ssh-public-keys 
   # list the user's SSH public keys 
   aws iam delete-ssh-public-key
   # delete SSH public key
   ```

5. Delete the user's Git credentials.

   ```bash
   aws iam list-service-specific-credentials 
   # list the user's git credentials
   aws iam delete-service-specific-credential
   ```

6. Deactivate the user's multi-factor authentication (MFA) device, if the user has one.

   ```bash
   aws iam list-mfa-devices 
   # list the user's MFA devices
   aws iam deactivate-mfa-device 
   # deactivate the device
   aws iam delete-virtual-mfa-device 
   # permanently delete a virtual MFA device
   ```

7. Delete the user inline policies.

   ```bash
   aws iam list-user-policies 
   # list the inline policies for the user
   aws iam delete-user-policy 
   # delete the policy
   ```

8. Detach any managed policies that are attached to the user.

   ```bash
   aws iam list-attached-user-policies 
   # list the managed policies attached to the user  
   aws iam detach-user-policy 
   # detach the policy
   ```

9. Remove the user from any user groups.

   ```bash
   aws iam list-groups-for-user 
   # list the user groups to which the user belongs
   aws iam remove-user-from-group
   ```

10. Delete the user.
    ```bash
    aws iam delete-user
    ```

#### Add Restrictive Policy for IP

Create an identity-based policy that denies access to all AWS operations in the account if the request originates from principals outside the specified IP ranges. This policy is useful when the IP addresses of your organization fall within the specified ranges. In this example, access will be denied unless the request comes from the CIDR ranges 192.0.2.0/24 or 203.0.113.0/24.

##### AWS Management Console

1. Sign in to the AWS  and open the [IAM Console](https://console.aws.amazon.com/iam/). 

2. On the navigation pane, select **Users**.

3. Choose the user you want to deactivate.

4. Select the **Permissions** tab and click **Add inline policy**.

5. In the JSON tab, enter the following policy to deny all actions:

   ```json
   {
       "Version": "2012-10-17",
       "Statement": {
           "Effect": "Deny",
           "Action": "*",
           "Resource": "*",
           "Condition": {
               "NotIpAddress": {
                   "aws:SourceIp": [
                       "192.0.2.0/24",
                       "203.0.113.0/24"
                   ]
               }
           }
       }
   }
   ```

6. Review the policy and click **Create policy**.

##### AWS CLI

1. Open the terminal or a command prompt.

2. Create a JSON file, for example `deny_all_source_policy.json`,  with the following content:

   ```json
   {
       "Version": "2012-10-17",
       "Statement": {
           "Effect": "Deny",
           "Action": "*",
           "Resource": "*",
           "Condition": {
               "NotIpAddress": {
                   "aws:SourceIp": [
                       "192.0.2.0/24",
                       "203.0.113.0/24"
                   ]
               }
           }
       }
   }
   ```

3. Run the following command to attach the policy to the IAM user:

   ```bash
   aws iam put-user-policy --user-name <USERNAME> --policy-name DenyAllSourcePolicy --policy-document file://deny_all_source_policy.json
   ```


   Replace <USERNAME> with the actual username.

#### Force Password Reset

##### AWS Management Console

1. Sign in to the AWS  and open the [IAM Console](https://console.aws.amazon.com/iam/). 
2. On the navigation pane, select **Users**.
3. Choose the user you want to force password reset.
4. Select the **Security credentials** tab.
5. Click **Manage console access** and select **Reset password**.
6. Select  **User must create new password at next sign-in**.
7. Click **Reset password**.

##### AWS CLI

1. Open the terminal or a command prompt.

2. Run the following command to require a password reset:

   ```bash
   aws iam update-login-profile --user-name <USERNAME> --password-reset-required
   ```

​	Replace <USERNAME> with the actual username.

#### Delete and Create New Access Keys

##### AWS Management Console

1. Sign in to the AWS  and open the [IAM Console](https://console.aws.amazon.com/iam/). 
2. On the navigation pane, select **Users**.
3. Choose the user you want to force password reset.
4. Select the **Security credentials** tab and locate the **Access keys** section.
5. Select **Delete** from the **Actions** drop-down.
6. Click on **Deactivate**.
7. To confirm deletion, enter the access key ID in the text input field, and select **Delete**. 
8. Select the **User must create new password at next sign-in**.
9. Click **Reset password**.

To create a new access key:

1. Click **Create access key**. 

2. Click **Download .csv file** or **Show** to save the new access key securely. 

   Make sure that you store the new access key and secret access key in a secure place, as you will not be able to view the secret access key again.

##### AWS CLI

1. Open your terminal or command prompt.

2. List the current access keys for the user to identify which keys exist:

   ```bash
   aws iam list-access-keys --user-name <USERNAME>
   Replace <USERNAME> with the actual username.
   ```

3. Delete an existing access key:

   ```bash
   aws iam delete-access-key --user-name <USERNAME> --access-key-id <ACCESSKEYID>
   ```

   Replace <USERNAME> with the actual username and <ACCESSKEYID> with the ID of the access key you want to delete.

4. Create a new access key:

   ```bash
   aws iam create-access-key --user-name <USERNAME>
   ```

   Replace <USERNAME> with the actual username.
