# Configuring a named profile to use IAM Identity Center

### Prerequisites
* The account which will be accessed must be available on the AWS IAM Identity Center portal

![image](https://github.com/fabbriciocruz/AWS_CLI_Authentication_Methods/blob/6aaac5b21d36275e783ea8912f41553dc4876362/Images/AwsSSOPortal.png)

1. Edit the .aws/config and add the following (Replace the values between <> as you need)

    ```sh
    [profile <MY_AWSCLI_PROFILE_NAME>]
    sso_start_url = <AWS_SSO_PORTAL_URL>
    sso_region = <The Aws region where the SSO has been configured>
    sso_account_id = <The AWS account ID that contains the IAM role that you want to use with this profile>
    sso_role_name = <The name of the IAM role that defines the user's permissions when using this profile>
    region = <The main Aws region where you will work>
    output = json
    ```

2. After you configure a named profile, you can invoke it to request temporary credentials from AWS. Before you can run an AWS CLI service command, you must retrieve and cache a set of temporary credentials. To get these temporary credentials, run the following command and replace the value between <>

    ```sh
    aws sso login --profile <MY_AWSCLI_PROFILE_NAME>
    ```

3. Run the following command to check if you've done everything all right

    ```sh
    aws sts get-caller-identity --profile <MY_AWSCLI_PROFILE_NAME>
    ```

    * When the credential expires you'll need to run the aws sso login command again

    ### Tips
    * To access Aws CodeCommit you just need to run this how-to and then clone your CodeCommit repository running the following command:

        ```sh
        git clone codecommit::<YOUR REPOSITORY REGION>://<MY_AWSCLI_PROFILE_NAME>@<YOUR REPOSITORY NAME>
        ```

### References
* [Configuring the AWS CLI to use AWS IAM Identity Center (successor to AWS Single Sign-On)](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html)


* [Federated multi-account access for AWS CodeCommit
](https://aws.amazon.com/blogs/devops/federated-multi-account-access-for-aws-codecommit/)