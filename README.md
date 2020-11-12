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
    - EXTRA_VARS: (List)
      - KEY: (String)
      - VALUE: (String) (Allow CloudFormation variables)
      - SUBVALUE: (String Optional) (Allow CloudFormation variables. Used when you need to use an intrinsic function in the VALUE, like Fn::ImportValue)
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
            TEMPLATE_DEST: ./artifacts/{{EnvironmentType}}/cf-elasticstack-ssm.yml
            TAGS:
              RELEASE: "{{Release}}"
              ENVIRONMENT_TYPE: "{{EnvironmentType}}"
            ASSOCIATION
            - NAME: MasterNode{{ReleaseName}}
              EXTRA_TARGETS:
              - KEY: tag:ES-MASTER
                VALUES:
                - true
              DOCUMENT_NAME: AWS-ApplyAnsiblePlaybooks
              PARAMETERS:
                SOURCE_TYPE: S3
                INSTALL_DEPENDENCIES: False
                PLAYBOOK_FILE: deploy-node-prod.yml
                EXTRA_VARS:
                - KEY: DISCOVERY_SEED_PROVIDERS
                  VALUE: ec2
                - KEY: DISCOVERY_EC2_TAG_NAME
                  VALUE: ClusterName
                - KEY: DISCOVERY_EC2_TAG_VALUE
                  VALUE: ${ClusterName}
                - KEY: CODEBUILD_BUCKET
                  VALUE: BucketName
                  SUBVALUE: "Fn::ImportValue: !Sub ${ClusterName}-ArtifactsBucket"
                CHECK: False
                VERBOSE: -v
          
License
-------

BSD

Author Information
------------------

Moegui.com
