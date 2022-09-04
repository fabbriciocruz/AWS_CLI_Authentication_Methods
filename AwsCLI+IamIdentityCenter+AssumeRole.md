# AWS CLI + IAM Identity Center + Assume Role

### Retrieving short-term credentials for CLI use with AWS IAM Identity Center + Assume Role


1. Go to the AWS IAM Identity Center portal (Replace XXXX by the URL of your organization)
https://XXXX.awsapps.com/start#/

2. Choose “AWS Account” to expand the list of AWS accounts

![image](https://github.com/fabbriciocruz/AWS_CLI_Authentication_Methods/blob/2bddf35c2ca3120b9209ea5c5e8d8f48b53e3500/Images/expand_aws_accounts.png)

3. Choose the AWS account that you want to access using the AWS CLI. This expands the list of permission sets in the account. Then choose "Command line or programatic access"

![image](https://github.com/fabbriciocruz/AWS_CLI_Authentication_Methods/blob/2bddf35c2ca3120b9209ea5c5e8d8f48b53e3500/Images/expand_permission_sets.png)

4. Move your mouse over the option you want to copy credentials <br >
Copy the varibles and paste it on your terminal

![image](https://github.com/fabbriciocruz/AWS_CLI_Authentication_Methods/blob/2bddf35c2ca3120b9209ea5c5e8d8f48b53e3500/Images/move_mouse_over.png)

5. Optionally, you can verify that the credentials are set up correctly by running the “aws configure list” command.

![image](https://github.com/fabbriciocruz/AWS_CLI_Authentication_Methods/blob/2bddf35c2ca3120b9209ea5c5e8d8f48b53e3500/Images/aws_configure_list.png)

6. Run the following command and replace the values between <>

    ```sh
    eval $(aws sts assume-role \
    --role-arn arn:aws:iam::<ACCOUNT_ID>:role/<IAM_ROLE_NAME> \
    --role-session-name <SESSTION_NAME> \
    | jq -r '.Credentials \
    | "export AWS_ACCESS_KEY_ID=\(.AccessKeyId)\nexport AWS_SECRET_ACCESS_KEY=\(.SecretAccessKey)\nexport AWS_SESSION_TOKEN=\(.SessionToken)\n"')
    ```