# Configure Config Aggregator
Orginal Created by: cornickj@
Orginal Curated by: ajmorga@
Customized by: kyeonjoo@

The Landing Zone deploys a starter set of Config Rules, but there is no central view.  Enabling Config Aggregator in the Security account will allow the customer GRC teams to see a cross-account view of compliance.

This is done by deploying a Config Aggregator in the Security account, and an AggregationAuthorization in each other account.  (Much simpler than GuardDuty and their invite/accept workflow!)

Then, the config aggregator in the security account needs to be updated to know about all the other accounts, so an update process runs in the Organization Root that queries the list of Active accounts and updates the Config Aggregator in the Security account each day.  It will then poll those accounts for compliance and provide the aggregate view we all need.

## Instructions

* Copy the configaggregator directory into your landing zon configuration add-on directory.
* Edit the user-input.yaml file, and change the StateMachineLambdaRole parameter value so it has your master account id in the ARN.

## Original Instructions

In addition, the default config uses dynamic naming for config rules.  Generally that is a cloudformation best practice, but it causes issues when consolidating the results, so I like to update that config template with one that has static names for the rules.  I do append a version so if the rules are changed that name should be changed, allowing replacement in stack updates, the revised template is here, just overwrite the one in your /templates/aws_baseline folder if you haven't made other changes. 

To deploy:
    Download your aws-landing-zone-configuration.zip from the S3 bucket created by the Landing Zone and do the following modifications:
        Download the Config Aggregator template and save it in /templates/aws_baseline
        Download the baseline account parameter file and save it in /parameters/aws_baseline
        Download the Config Aggregator Service template and save it in /templates/core_accounts
        Download the Config Aggregator parameter file and save it in /parameters/core_accounts
        Update the StateMachineLambdaRole parameter to reflect the actual Arn of the StateMachineLambdaRole in the Landing Zone Primary account.  It will have a dynamic name created from the Stack name and isn't available as a parameter.
    Edit the manifest.yaml:
        Add a block under baseline_resources (towards the end of the file):
            - name: ConfigAggregator
            baseline_products:
              - AWS-Landing-Zone-Account-Vending-Machine
            template_file: templates/aws_baseline/aws-landing-zone-config-aggregator.template
            parameter_file: parameters/aws_baseline/aws-landing-zone-config-aggregator.json
            deploy_method: stack_set
        In the primary account section (under the "core_accounts" section), uncomment the core_resources line, and add the following to core_resources:
        - name: ConfigAggregatorService
          template_file: templates/core_accounts/aws-landing-zone-config-aggregator-service.template
          parameter_file: parameters/core_accounts/aws-landing-zone-config-aggregator-service.json
          deploy_method: stack_set
    Zip the aws-landing-zone-configuration.zip file and upload it to S3 encrypted with the KMS key
    Wait for the Codepipeline to finish.