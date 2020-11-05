ASBRL-SSM-STATEMANAGER
=========

Create a CloudFormation Template to Deploy a SMM Stack

Requirements
------------

- jinja2_extensions = jinja2.ext.loopcontrols (Add This line in your [default] section of the ansible.cfg)


Role Variables
--------------

- TEMPLATE_DEST: (String)
- TAGS:
    - RELEASE: (String)
    - ENVIRONMENT_TYPE: (String)
- ASSOCIATION: (List)
  - NAME: (String)
  - EXTRA_TARGETS: (List)
    - Key: (String)
    - Value: (String)
  - DOCUMENT_NAME: (String)
  - PARAMETERS: (If DOCUMENT_NAME is AWS-ApplyAnsiblePlaybooks)
    - SOURCE_TYPE: (String)
    - INSTALL_DEPENDENCIES: (Boolean)
    - PLAYBOOK_FILE: (String)
     -EXTRA_VARS: (List)(Allow CloudFormation variables)
      - REGION=${AWS::Region}
    - CHECK: (Boolean)
    - VERBOSE: (String)
            
Dependencies
------------

None

Example Playbook
----------------

          
      - name: Generate {{EnvironmentType}} cf-ssm
        include_role:
          name: asbrl-ssm-statemanager
        vars:
          TEMPLATE_DEST: ./artifacts/{{EnvironmentType}}/cf-rabbitstack-ssm.yml
          TAGS:
            RELEASE: "{{Release}}"
            ENVIRONMENT_TYPE: "{{EnvironmentType}}"
          ASSOCIATION:
          - NAME: RabbitNodeV01
            EXTRA_TARGETS:
            DOCUMENT_NAME: AWS-ApplyAnsiblePlaybooks
            PARAMETERS:
              SOURCE_TYPE: S3
              INSTALL_DEPENDENCIES: False
              PLAYBOOK_FILE: deploy-node.yml
              EXTRA_VARS:
              - BUILD=${Build}
              - REGION=${AWS::Region}
              - LOGS_GROUP_NAME=${Prefix}-${ClusterName}
              - METRICS_NAMESPACE=${Prefix}-${ClusterName}
              - CLUSTER_NAME=${ClusterName}
              - PREFIX=${Prefix}
              - ACCOUNT_ID=${AWS::AccountId}
              - DISCOVERY=rabbit_peer_discovery_aws
              - USE_LONGNAME=true
              CHECK: False
              VERBOSE: -v
            
License
-------

BSD

Author Information
------------------

Moegui.com
