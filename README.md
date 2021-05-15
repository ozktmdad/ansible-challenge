# Ansible Technical Challenge

This test uses ansible to:
- Create VPC in AWS
- Deploy a minimum of two RHEL8 virtual machines in AWS VPC, you can add more by editing the one_page_challenge dictionary variable in group_vars/all.yml
- Use dynamic inventory to add new virtual machines to ansibles inventory
- Some basic configuration management to the systems has been deployed deploy (e.g. chrony, timezone, SSH hardening, installing packages that should be in a Linux SOE).
- The virtual machines have httpd installed and using self signing certificates to host a different html file from https://onehtmlpagechallenge.com/:
  - Strange Insults
	- A Tribute Page
  (you can add more or change onepagehtmlpagechallenge by editing the one_page_challenge dictionary variable in group_vars/all.yml)
- Playbooks/roles should be idempotent

## Prerequisites

# Software Prerequsites (as tested)
Lower levels of software may work.  These versions were used and tested by the developer.

ansible >= 2.9.21

- python requirements, install via pip or use
To install python requirements please ins
```
python -m pip install -r requirements.txt
```
# AWS Account Prerequiste 
Have configured AWS aws_access_key_id and secret access key IAM access abitiy to create, VPC and EC2instances.  (If you don't know how to do this you probably shouldn't be assessing me)

## Clone
- Clone this repo to your local machine using 
git clone https://github.com/ozktmdad/ansible-challenge.git

## Setup

# Configure password vault

- Create vault password similar to ssh key to encrypt your usernames and passwords in playbook
```shell
$ echo "myvaultkey" > ~/ansible_challenge.vault
```

Please use a stronger key than "myvaultkey" I would recommend using a 2048 bit key as it makes me feel like it is actually secure.

- Create two encrypted passwords using ansible-vault and techchallenge vault
1. aws_access_key_id
2. aws_secret_access_key

```shell
$ ansible-vault encrypt_string "AZRXC5HHHBYVAKDAFJ4" --vault-password-file ~/techchallenge.vault
!vault |
          $ANSIBLE_VAULT;1.1;AES256
          38643064656331346431393332366133306439663532626131613532646138376165306431633937
          3330646664333730346462383262313631376664643366620a666231326637343830306532303531
          65393966616132373234323739616338313837316162343565346562316138323338336234353062
          3066376364383034310a643863623038353765316666393065373266386438643938333061313331
          63376330373861323537303737343134623962376663366262653235343366386630
Encryption successful
$ ansible-vault encrypt_string "ajdfkakfak4r4r4rnkafa6767jfafk" --vault-password-file ~/techchallenge.vault
!vault |
          $ANSIBLE_VAULT;1.1;AES256
          39666536353561636437613131626236343864343064666538363964656662313534373761366461
          3639663762646538373332323439643261356232366131650a376632613636643665313737636162
          63323961306437346532636134383761313931663830303736663362373836646165313736616361
          6633616337613930650a333062653136636437346236653232303361336166383232373831356662
          66303661376562623136313465623765643938346431386564656335386562613365
Encryption successful
```
# Update encrypted passwords in the app
The access_key and secret key need to be updated in two places.  Yeah I know this is bad, but the inventory just didn't like using global variables.

Open file group_vars/all.yml and update variables:
- aws_access_key
- aws_secret_key

Open file inventory/ap-southeast-2_aws_ec2.yml and update variables:
- aws_access_key
- aws_secret_key

## Usage
After all the prereq work has been done, your ready to run.

Run ansible playbook to deploy the solution:

```
ansible-playbook -i inventory/ap-southeast-2_aws_ec2.yml site.yml --vault-password-file ~/ansible_challenge.vault
```

Any questions please feel free to contact me.  Architectural decisions have not been included so please feel free to ask.

## Bonus Points:
Insult Generater: yes
Run it is ECS Container: no
  If you want me to do it to prove that I can do it, please contact me.
