level27tech.aws_nacl
==========

This role creates, updates or deletes an Amazon Web Services (AWS) network access control list (NACL).

The object will be created/updated to include a Name tag and a chosen Project tag.

The ACL is identified by VPC and ACL name and so both must be specified.

Requirements
------------------

In order to use this role you need an AWS account and an AWS user with appropriate permissions. 
To make things easier an AWS policy document is included with this role.

>AnsibleNACLPolicy.json

As this role interfaces with the AWS API, the boto Python module is also required.
One way to install boto is to use pip:
> pip install boto

Role Variables
-------------------

**AWS Credentials**

*Note that these credentials should be kept private as they can be used to gain access to your AWS environment.
It is recommended that these variables be encrypted with Ansible Vault.  See https://docs.ansible.com/ansible/2.4/ansible-vault.html for details.*

aws_nacl_aws_access_key: MySecretKeyABC123
aws_nacl_aws_secret_key: MyAccessKeyABC123

**AWS Region**

*The region in which to create the Network ACL*

aws_nacl_region: us-east-1

** Network ACL name **

aws_nacl_name: acl-network-access

** VPC Name **

aws_nacl_vpc_name: my-vpc

** Purge Subnets **

*Remove existing subnets when updating?*

aws_nacl_purge_subnets: True

** Purge ingress rules **

*Remove existing ingress rules when updating?*

aws_nacl_purge_ingress_rules: True

** Purge egress rules **

*Remove existing egress rules when updating?*

aws_nacl_purge_egress_rules: True

** Subnets to associate to the network ACL **

aws_nacl_subnets: []

** Ingress rules for the network ACL **

aws_nacl_ingress_rules: []

** Egress rules for the network ACL **

aws_nacl_egress_rules: []

** The Project tag **

aws_nacl_project: MyProject

** The state for the network ACL. **

*Set to "absent" for deletion*

aws_nacl_state: present

The role includes no variables (vars).
The role does not depend on any global variables or variables from other roles.

Dependencies
------------------

This role does not depend on any other Ansible roles.

Example Playbook
----------------

The following example would create a network ACL.

    - hosts: localhost
      connection: local
      gather_facts: False
      tasks:
      - include_role:
          name: level27tech.aws_nacl
      vars:
        aws_nacl_aws_access_key: "AVIBI4QDEWFZOC2T3K4A"
        aws_nacl_aws_secret_key: "Br9t-/gEN+OYfrbDmr7f63NyliCPIrDvTdcTUMHf"
        aws_nacl_region: "us-east-1"
        aws_nacl_name: "acl_test"
        aws_nacl_vpc_name: "vpc_test"
        aws_nacl_project: "Test-project"
        aws_nacl_state: "present"
        aws_nacl_subnets:
        - "sub_public_a"
        aws_nacl_ingress_rules:
        - [100,'tcp','allow','192.168.0.1/32', null, null, 22, 22]
        - [110,'tcp','allow','0.0.0.0/0', null, null, 1024, 65535]
        aws_nacl_egress_rules:
        - [100,'tcp','allow','192.168.0.1/32', null, null, 1024, 65535]
        - [110,'tcp','allow','0.0.0.0/0', null, null, 80, 80]
        - [120,'tcp','allow','0.0.0.0/0', null, null, 443, 443]
          
The following example would delete a network ACL.

    - hosts: localhost
      connection: local
      gather_facts: False
      tasks:
      - name: Delete Network ACL
        include_role:
          name: level27tech.aws_nacl
      vars:
        aws_nacl_aws_access_key: "AVIBI4QDEWFZOC2T3K4A"
        aws_nacl_aws_secret_key: "Br9t-/gEN+OYfrbDmr7f63NyliCPIrDvTdcTUMHf"
        aws_nacl_region: "us-east-1"
        aws_nacl_name: "acl_test"
        aws_nacl_vpc_name: "vpc_test"
        aws_nacl_project: "Test-project"
        aws_nacl_state: "absent"
          
License
----------

GPLv3.0

Author Information
---------------------------

Name: Dean Harris

Organisation: Level 27 Technology Ltd
