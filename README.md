
## Prerequisites

* Java    (sudo yum install java-1.8.0-openjdk)
* Install brew (https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-install-linux-alt.html)
* [Leiningen](http://leiningen.org/) (can be installed using `brew install leiningen`)
* AWS Account
* AWS cli configured on your system with admin privleges
* Python 2.7/3.6/3.7
* Boto3 package (`pip install boto3`)

## Depoy IaaC


   ```
   $ aws cloudformation deploy \
   --template-file infrastructure/infrastructure.yml \
   --region <region> \
   --stack-name <stack name> \
   --capabilities CAPABILITY_NAMED_IAM
   ```
2. Build applications

    ```
    make libs
    make clean all
    ```
3. Login to ECR

    ```
    $ $(aws ecr get-login --no-include-email --region <region>)
    ```

4. Deploy Quotes and Newsfeed services as containers onto your cluster: 

   ```
   $ ./deploy.py <stack_name> <service name> <region>
   ```
5. Prereq before pushing Frontend service:

    ```
    open services/front-end/Dockerfile
    replace the URL for quotes and newsfeed with the loadbalancer url created after executing cloudformation template in Step 1

    ```
6.  Build & deploy Frontend service:

    ```
    $ ./deploy.py <stack_name> frontend <region>
    ```
