[![FivexL](https://releases.fivexl.io/fivexlbannergit.jpg)](https://fivexl.io/)

# terraform-aws-ecs-events-to-slack
Rules for Amazon EventBridge that fetch ECS events and send them to Slack, Teams and Chime

# AWS Chatbot Integration for Slack, Teams, and Chime

This module provides integration between AWS services and various chat platforms (Slack, Microsoft Teams, and Amazon Chime) using AWS Chatbot.

## Features

- Support for multiple chat platforms:
  - Slack (fully tested and supported)
  - Microsoft Teams (requires paid Microsoft 365 account, configuration not fully tested)
  - Amazon Chime (no Terraform configuration available yet)
- AWS Chatbot integration for seamless communication
- Event-driven notifications from AWS services
- Customizable message formatting
- Built-in EventBridge rules for ECS events
- SNS topic for message delivery
- Configurable IAM roles and policies

## Resources

This module creates the following resources:

- AWS Chatbot configuration for Slack/Teams
- Lambda function for processing events
- EventBridge rules for monitoring AWS services
- SNS topic for message delivery
- IAM roles and policies for Lambda and Chatbot

## Prerequisites

- AWS Account
- Slack workspace (for Slack integration)
- Microsoft Teams (for Teams integration)
- Amazon Chime (for Chime integration)
- AWS Chatbot configured in your AWS account

## Usage

### Slack Integration

```hcl
module "chatbot" {
  source = "path/to/module"

  name = "my-chatbot"
  
  slack_config = {
    channel_id   = "YOUR_SLACK_CHANNEL_ID"
    workspace_id = "YOUR_SLACK_WORKSPACE_ID"
  }
}
```

### Microsoft Teams Integration

```hcl
module "chatbot" {
  source = "path/to/module"

  name = "my-chatbot"
  
  teams_config = {
    team_id         = "YOUR_TEAMS_ID"
    channel_id      = "YOUR_TEAMS_CHANNEL_ID"
    teams_tenant_id = "YOUR_TEAMS_TENANT_ID"
  }
}
```

⚠️ **Important Notes**:
- Microsoft Teams integration requires a paid Microsoft 365 account
- Teams configuration is not fully tested due to account requirements
- Amazon Chime integration is planned but not yet implemented in Terraform

## AWS Chatbot Setup

1. Configure AWS Chatbot in your AWS Console:
   - Go to AWS Chatbot console
   - Choose your chat platform (Slack/Teams/Chime)
   - Follow the setup wizard to connect your workspace/team
   - Note down the required IDs (channel_id, workspace_id, etc.)

2. Required Permissions:
   - AWS Chatbot needs appropriate IAM roles
   - Chat platforms need proper access tokens
   - SNS topics need correct policies

3. Platform-specific Requirements:
   - Slack: Requires workspace admin permissions
   - Teams: Requires Microsoft 365 admin access
   - Chime: Requires AWS account permissions

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Example
```hcl
module "ecs_to_slack" {
  source            = "git::https://github.com/fivexl/terraform-aws-ecs-events-to-slack.git"
  name              = "amazon_q_notifications"

  # Enable ECS task state change events
  enable_ecs_task_state_event_rule = true
  ecs_task_state_event_rule_detail = {
    clusterArn = ["arn:aws:ecs:us-east-1:123333333333:cluster/your-cluster-name"]
  }
  slack_config = {
    channel_id   = "1234567890"  # Your Slack workspace ID
    workspace_id = "1234567890"   # Your Slack channel ID
  }
  #Testing is required for teams
  # teams_config = {
  #   team_id         = "1234567890" # Your Teams id ID
  #   channel_id      = "1234567890" # Your Teams channel ID
  #   teams_tenant_id = "1234567890" # Your Teams tenant ID
  # }


}

```
You can find more examples in the [`examples/`](./examples/) directory

## Info
- [Amazon ECS events](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_cwe_events.html)
- [Handling events with Lambda](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_cwet_handling.html)
- [EventBridge Patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns.html)
- [Amazon_q custom-notifs](https://docs.aws.amazon.com/chatbot/latest/adminguide/custom-notifs.html)

## AWS Terraform provier versions

* version 0.1.2 is the last version that works with both Terraform AWS provider v3 and v4. There are no plans to update 0.1.X branch.
* all versions later (0.2.0 and above) require Terraform AWS provider v4 as a baseline

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Requirements

| Name                                                                      | Version   |
| ------------------------------------------------------------------------- | --------- |
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 0.13.1 |
| <a name="requirement_aws"></a> [aws](#requirement\_aws)                   | >= 3.69   |

## Providers

| Name                                              | Version |
| ------------------------------------------------- | ------- |
| <a name="provider_aws"></a> [aws](#provider\_aws) | >= 3.69 |

## Modules

| Name                                                                                            | Source                           | Version |
| ----------------------------------------------------------------------------------------------- | -------------------------------- | ------- |
| <a name="module_slack_notifications"></a> [slack\_notifications](#module\_slack\_notifications) | terraform-aws-modules/lambda/aws | 5.0.0   |

## Resources

| Name                                                                                                                                                                                    | Type        |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| [aws_cloudwatch_event_rule.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_event_rule)                                                     | resource    |
| [aws_cloudwatch_event_target.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_event_target)                                                 | resource    |
| [aws_sns_topic.prod_chatbot](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sns_topic)                                                                     | resource    |
| [aws_sns_topic_policy.prod_chatbot](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sns_topic_policy)                                                       | resource    |
| [awscc_chatbot_slack_channel_configuration.this](https://registry.terraform.io/providers/hashicorp/awscc/latest/docs/resources/chatbot_slack_channel_configuration)                     | resource    |
| [awscc_chatbot_microsoft_teams_channel_configuration.this](https://registry.terraform.io/providers/hashicorp/awscc/latest/docs/resources/chatbot_microsoft_teams_channel_configuration) | resource    |
| [aws_caller_identity.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/caller_identity)                                                           | data source |
| [aws_region.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/region)                                                                             | data source |

## Inputs

| Name                                                                                                                                                           | Description                                                                                                                                                                                     | Type     | Default                                                                                                                                                                                                                          | Required |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------: |
| <a name="input_cloudwatch_logs_retention_in_days"></a> [cloudwatch\_logs\_retention\_in\_days](#input\_cloudwatch\_logs\_retention\_in\_days)                  | Specifies the number of days you want to retain log events in the specified log group. Possible values are: 1, 3, 5, 7, 14, 30, 60, 90, 120, 150, 180, 365, 400, 545, 731, 1827, and 3653.      | `number` | `14`                                                                                                                                                                                                                             |    no    |
| <a name="input_custom_event_rules"></a> [custom\_event\_rules](#input\_custom\_event\_rules)                                                                   | A map of objects representing the custom EventBridge rule which will be created in addition to the default rules.                                                                               | `any`    | `{}`                                                                                                                                                                                                                             |    no    |
| <a name="input_ecs_deployment_state_event_rule_detail"></a> [ecs\_deployment\_state\_event\_rule\_detail](#input\_ecs\_deployment\_state\_event\_rule\_detail) | The content of the `detail` section in the EvenBridge Rule for `ECS Deployment State Change` events. Use it to filter the events which will be processed and sent to Slack.                     | `any`    | <pre>{<br>  "eventType": [<br>    "ERROR"<br>  ]<br>}</pre>                                                                                                                                                                      |    no    |
| <a name="input_ecs_service_action_event_rule_detail"></a> [ecs\_service\_action\_event\_rule\_detail](#input\_ecs\_service\_action\_event\_rule\_detail)       | The content of the `detail` section in the EvenBridge Rule for `ECS Service Action` events. Use it to filter the events which will be processed and sent to Slack.                              | `any`    | <pre>{<br>  "eventType": [<br>    "WARN",<br>    "ERROR"<br>  ]<br>}</pre>                                                                                                                                                       |    no    |
| <a name="input_ecs_task_state_event_rule_detail"></a> [ecs\_task\_state\_event\_rule\_detail](#input\_ecs\_task\_state\_event\_rule\_detail)                   | The content of the `detail` section in the EvenBridge Rule for `ECS Task State Change` events. Use it to filter the events which will be processed and sent to Slack.                           | `any`    | <pre>{<br>  "lastStatus": [<br>    "STOPPED"<br>  ],<br>  "stoppedReason": [<br>    {<br>      "anything-but": {<br>        "prefix": "Scaling activity initiated by (deployment ecs-svc/"<br>      }<br>    }<br>  ]<br>}</pre> |    no    |
| <a name="input_enable_ecs_deployment_state_event_rule"></a> [enable\_ecs\_deployment\_state\_event\_rule](#input\_enable\_ecs\_deployment\_state\_event\_rule) | The boolean flag enabling the EvenBridge Rule for `ECS Deployment State Change` events. The `detail` section of this rule is configured with `ecs_deployment_state_event_rule_detail` variable. | `bool`   | `true`                                                                                                                                                                                                                           |    no    |
| <a name="input_enable_ecs_service_action_event_rule"></a> [enable\_ecs\_service\_action\_event\_rule](#input\_enable\_ecs\_service\_action\_event\_rule)       | The boolean flag enabling the EvenBridge Rule for `ECS Service Action` events. The `detail` section of this rule is configured with `ecs_service_action_event_rule_detail` variable.            | `bool`   | `true`                                                                                                                                                                                                                           |    no    |
