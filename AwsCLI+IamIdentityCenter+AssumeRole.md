# AWS CLI + IAM Identity Center + Assume Role

### Retrieving short-term credentials for CLI use with AWS IAM Identity Center + Assume Role

* There are two ways to configure the AWS CLI (Assume Role):
    * Temporary credentials valid only for one terminal session
    * Configure an AWS CLI named profile

## (Method 01) Temporary credentials valid only for one terminal session
* Tip: This approach will provide valid credential token as long as the your terminal session is openned. If you close your terminal sesstion and then open a new one you need to run all the following steps again (This appraoch is recommended when you eventually do some tasks on one of your organization's account)

1. Go to the AWS IAM Identity Center portal (Replace XXXX by the URL of your organization)
https://XXXX.awsapps.com/start#/

2. Choose “AWS Account” to expand the list of AWS accounts

![image](https://github.com/fabbriciocruz/AWS_CLI_Authentication_Methods/blob/2bddf35c2ca3120b9209ea5c5e8d8f48b53e3500/Images/expand_aws_accounts.png)

3. Choose the AWS account that you want to access using the AWS CLI. This expands the list of permission sets in the account. Then choose "Command line or programatic access"

![image](https://github.com/fabbriciocruz/AWS_CLI_Authentication_Methods/blob/2bddf35c2ca3120b9209ea5c5e8d8f48b53e3500/Images/expand_permission_sets.png)

4. Move your mouse over the "Option 1"

![image](https://github.com/fabbriciocruz/AWS_CLI_Authentication_Methods/blob/2bddf35c2ca3120b9209ea5c5e8d8f48b53e3500/Images/move_mouse_over.png)

5. Paste it on your terminal and press Enter

![image](https://github.com/fabbriciocruz/AWS_CLI_Authentication_Methods/blob/6d3d72bec8a5a137f9061ada5c7c6e643ae37251/Images/paste_on_your_terminal.png)

6. Optionally, you can verify that the credentials are set up correctly by running the “aws configure list” command.

![image](https://github.com/fabbriciocruz/AWS_CLI_Authentication_Methods/blob/2bddf35c2ca3120b9209ea5c5e8d8f48b53e3500/Images/aws_configure_list.png)

7. Run the following command replacing the values as indicated below

    ```sh
    eval $(aws sts assume-role --role-arn arn:aws:iam::ACCOUNT_ID:role/IAM_ROLE_NAME --role-session-name SESSION_NAME | jq -r '.Credentials | "export AWS_ACCESS_KEY_ID=\(.AccessKeyId)\nexport AWS_SECRET_ACCESS_KEY=\(.SecretAccessKey)\nexport AWS_SESSION_TOKEN=\(.SessionToken)\n"')
    ```

        Replace the values above as you need:
        ACCOUNT_ID: The AWS account ID which you want to switch role to
        IAM_ROLE_NAME: The IAM role you want to switch role to
        SESSION_NAME: Name for this session

8. Check if everything is ok running the following command (It'll list the S3 buckets from the account you want to switch role to as long as you have permissions to do it :)

    ```sh
    aws s3 ls --profile MY_AWS_CLI_PROFILE_NAME
    ```

## (Method 02) Configure an AWS CLI named profile
* Tip: This approach will provide valid credential token as long as the your terminal session is openned. If you close your terminal sesstion and then open a new one you need to run all the following steps again (This appraoch is recommended when you work eventually on some account)

1. Go to the AWS IAM Identity Center portal (Replace XXXX by the URL of your organization)
https://XXXX.awsapps.com/start#/

2. Choose “AWS Account” to expand the list of AWS accounts

![image](https://github.com/fabbriciocruz/AWS_CLI_Authentication_Methods/blob/6d3d72bec8a5a137f9061ada5c7c6e643ae37251/Images/expand_aws_accounts.png)

3. Choose the AWS account that you want to access using the AWS CLI. This expands the list of permission sets in the account. Then choose "Command line or programatic access"

![image](https://github.com/fabbriciocruz/AWS_CLI_Authentication_Methods/blob/2bddf35c2ca3120b9209ea5c5e8d8f48b53e3500/Images/expand_permission_sets.png)

4. Move your mouse over the Option 2

![image](https://github.com/fabbriciocruz/AWS_CLI_Authentication_Methods/blob/6d3d72bec8a5a137f9061ada5c7c6e643ae37251/Images/move_mouse_over_option2.png)

5. In your terminal open the file ~/.aws/credentials and paste the information you've copied <br >
<strong>Tip:</strong> If you don't have the ~/.aws/ directory you must first run the "aws configure" command and just press Enter for all options

![image](https://github.com/fabbriciocruz/AWS_CLI_Authentication_Methods/blob/6d3d72bec8a5a137f9061ada5c7c6e643ae37251/Images/vi_aws_credentials.png)

![image](https://github.com/fabbriciocruz/AWS_CLI_Authentication_Methods/blob/6d3d72bec8a5a137f9061ada5c7c6e643ae37251/Images/paste_on_aws_credentials_file.png)

6. Save and exit the file

7. Open the file ~/.aws/config and paste the following <br >

    ```sh
    [profile MY_AWS_CLI_PROFILE_NAME]
    role_arn = arn:aws:iam::ACCOUNT_ID:role/IAM_ROLE_NAME
    source_profile = Account_PermissionSet_Copied_fromd_SSO_Portal
    ```
        Replace the values above as you need:
        MY_AWS_CLI_PROFILE_NAME: The Aws Cli profile name
        ACCOUNT_ID: The AWS account ID which you want to switch role to
        IAM_ROLE_NAME: The IAM role you want to switch role to
        SOURCE_PROFILE: Account_PermissionSet_Copied_fromd_SSO_Portal

8. Save and exit the file

9. Check if everything is ok running the following command (It'll list the S3 buckets from the account you want to switch role to as long as you have permissions to do it :)

    ```sh
    aws s3 ls --profile MY_AWS_CLI_PROFILE_NAME
    ```
    <strong>Tip:</strong> Everytime your credentials expire you'll need to copy new ones from the IAM Identity Center portal and paste it in the ~/.aws/credentials file replacing the old ones
