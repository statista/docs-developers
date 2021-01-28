#ATHENA FEDERATED QUERY ACCESS

!!!  warning "only supported in USE at the moment"
* check if Athena Engine 2.0. gets used (_Workgroup settings_)
* add datasource
* choose _MySQL_
* choose _Add Lambda_ -> use given Cloudformation Script

####_Parameters_:
* add IAM role "LambdaSecretsManagerAccess" to Lambda IAM Role
    ```
    {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Sid": "VisualEditor0",
              "Effect": "Allow",
              "Action": "secretsmanager:GetSecretValue",
              "Resource": "*"
          }
      ]
    }
    ```
* add IAM role "AthenaGetQueryStreamingResults" to Lambda IAM role
    ```  
    {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Sid": "VisualEditor0",
              "Effect": "Allow",
              "Action": "athena:GetQueryResultsStream",
              "Resource": "*"
          }
      ]
    }
    ```
* add athena connection string to lambda environment variables