# Two ways to configure the AWS CLI to use AWS IAM Identity Center

### Prerequisites
* The account which will be accessed must be available in the AWS SSO portal

![image](https://github.com/fabbriciocruz/AWS_CLI_Authentication_Methods/blob/6aaac5b21d36275e783ea8912f41553dc4876362/Images/AwsSSOPortal.png)

### Configuring a named profile to use IAM Identity Center

1. Edit the .aws/config and add the following (Replace the values between <> as you need)

    ```sh
    [profile <MY_PROFILE_NAME>]
    sso_start_url = <AWS_SSO_PORTAL_URL>
    sso_region = <The Aws region where the SSO has been configured>
    sso_account_id = <The AWS account ID that contains the IAM role that you want to use with this profile>
    sso_role_name = <The name of the IAM role that defines the user's permissions when using this profile>
    region = <The main Aws region where you will work>
    output = json
    ```

2. After you configure a named profile, you can invoke it to request temporary credentials from AWS. Before you can run an AWS CLI service command, you must retrieve and cache a set of temporary credentials. To get these temporary credentials, run the following command and replace the value between <>

    ```sh
    aws sso login --profile <MY_PROFILE_NAME>
    ```

3. Run the following command to check if you've done everything all right

    ```sh
    aws sts get-caller-identity --profile <MY_PROFILE_NAME>
    ```

    * When the credential expires you'll need to run the aws sso login command again


### Retrieving short-term credentials for CLI use with AWS IAM Identity Center (Swtich Role)

1. Go to the AWS IAM Identity Center portal (Replace <XXXX> by the URL of your organization)
https://<XXXX>.awsapps.com/start#/

2. Choose “AWS Account” to expand the list of AWS accounts

![image](expand sso accounts)

3. Choose the AWS account that you want to access using the AWS CLI. This expands the list of permission sets in the account. Then choose "Command line or programatic access"

![image](expand_permission_sets)

4. Move your mouse over the option you want to copy credentials <br >
Copy the varibles and paste it on your terminal

![image](move_mouse_over)

5. Optionally, you can verify that the credentials are set up correctly by running the “aws configure list” command.

![image](aws_configure_list)

6. Run the following command and replace the values between <>

    ```sh
    eval $(aws sts assume-role --role-arn arn:aws:iam::<ACCOUNT_ID>:role/<IAM_ROLE_NAME> --role-session-name <SESSTION_NAME> | jq -r '.Credentials | "export AWS_ACCESS_KEY_ID=\(.AccessKeyId)\nexport AWS_SECRET_ACCESS_KEY=\(.SecretAccessKey)\nexport AWS_SESSION_TOKEN=\(.SessionToken)\n"')
    ```

## Tips
* To access Aws CodeCommit you just need to run this how-to and then clone your CodeCommit repository running the following command:

    ```sh
    git clone codecommit::<YOUR REPOSITORY REGION>://<YOUR REPOSITORY NAME>
    ```

### References
* [Configuring the AWS CLI to use AWS IAM Identity Center (successor to AWS Single Sign-On)](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html)


