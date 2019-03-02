# Ansible playbook to spawn a MongoDB 3 node replicaset

This repository contain my first steps with ansible. To practice, I tried to spin up a 3 node MongoDB replicaset which uses Letsencrypt certificates to encrypt communication.

## Usage

### Prerequisites

#### install necessary tools

On each server, you need the following tools:

- Docker
- certbot
- pip
- letsencrypt certificate

#### define hosts

To use this playbook, you need to define a group named dbservers in your ansible hosts file:

```bash
[dbservers]
mongo1.example.com rs_index=rs1 mongodb_root_username=<root-user> mongodb_root_password=<root-password>
mongo2.example.com rs_index=rs2 mongodb_root_username=<root-user> mongodb_root_password=<root-password>
mongo3.example.com rs_index=rs3 mongodb_root_username=<root-user> mongodb_root_password=<root-password>
```

#### create MongoDB pem keyfile

Also, you have to create a pem keyfile which is used by MongoDB to establish a TLS connection with a client on each node:

```bash
cat /etc/letsencrypt/live/<hostname_of_your_mongo_node>/fullchain.pem /etc/letsencrypt/live/<hostname_of_your_mongo_node>/privkey.pem > /opt/mongodb/ssl/mongo.pem
```

#### create keyfile for internal authentication

You have to replace the dummy key with a real key in the file keyfile. You can generate one like this:

```bash
  openssl rand -base64 756
```

### Execute playbook

simply run

```bash
  ansible-playbook main.yml
```

After you've run the playbook, you will have a ready-to-user MongoDB Replicaset with a user admin.

## Roadmap

- In order to simplify certificate creation and tool setup, I will provide a playbook for these tasks. Stay tuned.
- Automate pem keyfile creation
