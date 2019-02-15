# Step 3: Add a Manual Approval Stage<a name="customize-ec2-multi-endpoints-approval"></a>

As a best practice, add a manual approval stage in front of your new production stage\.

1. In the upper left, choose **Edit**\.

1. In your pipeline diagram, between the Deploy and Prod deployment stages, choose **\+ Add stage**\.

1. On **Edit stage**, enter a stage name \(for example, **Approval**\), and then choose **\+ Add action group**\.

1. In **Action name**, enter a name \(for example, **Approval**\)\.

1. In **Approval type**, choose **Manual approval**\.

1. \(Optional\) Under **Configuration**, in **SNS Topic ARN**, choose the SNS topic that you have created and subscribed to\.

1. Choose **Add Action**\.

1. In the AWS CodePipeline pane, choose **Save pipeline change**, and then choose **Save change**\. View your updated pipeline\.

1. To submit your changes and start a pipeline build, choose **Release change**, and then choose **Release**\.