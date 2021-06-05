# Infrastructure Security in AWS CodeStar<a name="infrastructure-security"></a>

As a managed service, AWS CodeStar is protected by the AWS global network security procedures that are described in the [Amazon Web Services: Overview of Security Processes](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf) whitepaper\.

You use AWS published API calls to access AWS CodeStar through the network\. Clients must support Transport Layer Security \(TLS\) 1\.0 or later\. We recommend TLS 1\.2 or later\. Clients must also support cipher suites with perfect forward secrecy \(PFS\) such as Ephemeral Diffie\-Hellman \(DHE\) or Elliptic Curve Ephemeral Diffie\-Hellman \(ECDHE\)\. Most modern systems such as Java 7 and later support these modes\.

Requests must be signed by using an access key ID and a secret access key that is associated with an IAM principal\. Or you can use the [AWS Security Token Service](https://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) \(AWS STS\) to generate temporary security credentials to sign requests\.

 By default, AWS CodeStar does not isolate service traffic\. Projects created using AWS CodeStar are open to the public internet unless you manually modify the access settings through Amazon EC2, API Gateway or Elastic Beanstalk\. This is intentional\. You can modify the access settings in Amazon EC2, API Gateway, or Elastic Beanstalk to the degree you want, including preventing all internet access\. 

 AWS CodeStar does not provide support for VPC endpoints \(AWS PrivateLink\) by default, but you can configure that support directly on the project resources\. 