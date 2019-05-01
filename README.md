[![Build Status](https://travis-ci.org/cfn-modules/rds-aurora-serverless.svg?branch=master)](https://travis-ci.org/cfn-modules/rds-aurora-serverless)
[![NPM version](https://img.shields.io/npm/v/@cfn-modules/rds-aurora-serverless.svg)](https://www.npmjs.com/package/@cfn-modules/rds-aurora-serverless)

# cfn-modules: AWS RDS Aurora Serverless cluster

RDS Aurora Serverless cluster with secure firewall configuration, [encryption](https://www.npmjs.com/package/@cfn-modules/kms-key), multi AZ, auto scaling, backup enabled, and [alerting](https://www.npmjs.com/package/@cfn-modules/alerting).

## Install

> Install [Node.js and npm](https://nodejs.org/) first!

```
npm i @cfn-modules/rds-aurora-serverless
```

## Usage

```
---
AWSTemplateFormatVersion: '2010-09-09'
Description: 'cfn-modules example'
Resources:
  AuroraServerlessCluster:
    Type: 'AWS::CloudFormation::Stack'
    Properties:
      Parameters:
        VpcModule: !GetAtt 'Vpc.Outputs.StackName' # required
        ClientSgModule: !GetAtt 'ClientSg.Outputs.StackName' # required
        KmsKeyModule: !GetAtt 'Key.Outputs.StackName' # required
        BastionModule: !GetAtt 'Bastion.Outputs.StackName' # optional
        HostedZoneModule: !GetAtt 'HostedZone.Outputs.StackName' # optional
        AlertingModule: !GetAtt 'Alerting.Outputs.StackName' # optional
        SecretModule: !GetAtt 'Secret.Outputs.StackName' # optional
        Engine: 'aurora' # optional
        DBSnapshotIdentifier: '' # optional
        DBName: 'test' # required (ignored when DBSnapshotIdentifier is set, value used from snapshot)
        DBBackupRetentionPeriod: '30' # optional
        DBMasterUsername: 'master' # optional
        DBMasterUserPassword: 'SuP3rS3curE' # required (ignored when DBSnapshotIdentifier is set, value used from snapshot; also ignored if SecretModule is set)
        SubDomainNameWithDot: '' # optional
        PreferredBackupWindow: '09:54-10:24' # optional
        PreferredMaintenanceWindow: 'sat:07:00-sat:07:30' # optional
        AutoPause: 'true' # optional
        SecondsUntilAutoPause: '300' # optional
        MaxCapacity: '2' # optional
        MinCapacity: '2' # optional
      TemplateURL: './node_modules/@cfn-modules/rds-aurora-serverless/module.yml'
```

## Parameters

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Description</th>
      <th>Default</th>
      <th>Required?</th>
      <th>Allowed values</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>VpcModule</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/vpc">vpc module</a></td>
      <td></td>
      <td>yes</td>
      <td></td>
    </tr>
    <tr>
      <td>ClientSgModule</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/client-sg">client-sg module</a> where traffic is allowed from on port 5432 to the database</td>
      <td></td>
      <td>yes</td>
      <td></td>
    </tr>
    <tr>
      <td>KmsKeyModule</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/kms-key">kms-key module</a> (only works in combination with Access := [Private, PublicRead])</td>
      <td></td>
      <td>yes</td>
      <td></td>
    </tr>
    <tr>
      <td>BastionModule</td>
      <td>Stack name of <a href="https://www.npmjs.com/search?q=keywords:cfn-modules:Bastion">module implementing Bastion</a></td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>HostedZoneModule</td>
      <td>Stack name of <a href="https://www.npmjs.com/search?q=keywords:cfn-modules:HostedZone">module implementing HostedZone</a></td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>AlertingModule</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/alerting">alerting module</a></td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>SecretModule</td>
      <td>Stack name of <a href="https://www.npmjs.com/package/@cfn-modules/secret">secret module</a></td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>Engine</td>
      <td>The name of the database engine that you want to use for this DB cluster</td>
      <td>aurora</td>
      <td>no</td>
      <td>[aurora]</td>
    </tr>
    <tr>
      <td>DBSnapshotIdentifier</td>
      <td>Identifier for the DB cluster snapshot from which you want to restore (leave blank to create an empty cluster)/td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>DBName</td>
      <td>Name of the database (ignored when DBSnapshotIdentifier is set, value used from snapshot)</td>
      <td></td>
      <td>depends</td>
      <td></td>
    </tr>
    <tr>
      <td>DBBackupRetentionPeriod</td>
      <td>The number of days to keep snapshots of the cluster</td>
      <td>30</td>
      <td>no</td>
      <td>[1-35]</td>
    </tr>
    <tr>
      <td>DBMasterUsername</td>
      <td>The master user name for the DB instance (ignored when DBSnapshotIdentifier is set, value used from snapshot)</td>
      <td>master</td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>DBMasterUserPassword</td>
      <td>The master password for the DB instance (ignored when DBSnapshotIdentifier is set, value used from snapshot; also ignored if SecretModule is set)</td>
      <td></td>
      <td>depends</td>
      <td></td>
    </tr>
    <tr>
      <td>SubDomainNameWithDot</td>
      <td>Name that is used to create the DNS entry with trailing dot, e.g. §{SubDomainNameWithDot}§{HostedZoneName}. Leave blank for naked (or apex and bare) domain. Requires HostedZoneModule parameter!</td>
      <td>aurora.</td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>PreferredBackupWindow</td>
      <td>The daily time range in UTC during which you want to create automated backups</td>
      <td>09:54-10:24</td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>PreferredMaintenanceWindow</td>
      <td>The weekly time range (in UTC) during which system maintenance can occur</td>
      <td>sat:07:00-sat:07:30</td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>AutoPause</td>
      <td>The time, in seconds, before a Serverless Aurora cluster is paused</td>
      <td>300</td>
      <td>no</td>
      <td>[1-86400]</td>
    </tr>
    <tr>
      <td>SecondsUntilAutoPause</td>
      <td>Stack name of lambda-function module to receive events from this S3 bucket. Also grants the Lambda function access to this bucket and this bucket access to the Lambda function.</td>
      <td></td>
      <td>no</td>
      <td></td>
    </tr>
    <tr>
      <td>MaxCapacity</td>
      <td>The maximum capacity units for a Serverless Aurora cluster</td>
      <td>2</td>
      <td>no</td>
      <td>[1, 2, 4, 8, 16, 32, 64, 128, 256]</td>
    </tr>
    <tr>
      <td>MinCapacity</td>
      <td>The minimum capacity units for a Serverless Aurora cluster</td>
      <td>2</td>
      <td>no</td>
      <td>[1, 2, 4, 8, 16, 32, 64, 128, 256]</td>
    </tr>
  </tbody>
</table>
