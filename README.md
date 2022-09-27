
# TigerGraph Server Setup

TigerGraph, the world’s fastest and most scalable graph analytics platform, breaks through the limitations of other graph technologies to enable real-time big data graph applications. It also lowers cost by getting more done with less storage, less memory, and less time.


## Hardware Specifications

#### Single Node

| Installation Type | Instance Type |Description                       |Storage                       |
| :-------- | :------- | :------- |:------- |
| `Single Node` | `r4.xlarge` | 30.5 GiB of memory, 4 vCPUs, 64-bit platform | 200GB                |

#### Cluster Setup

| Installation Type | Instance Type |Description                       |Storage                       |
| :-------- | :------- | :------- |:------- |
| `Cluster` | `r4.2xlarge` | 61 GiB of memory, 8 vCPUs, 64-bit platform|≥ 1TB|

#### Operating System
Ubuntu 20.04 LTS


CPU, memory, storage to be resized on data size and application requirements


## Prerequisite Software

- crontab
- curl
- ip
- more
- netcat
- netstat
- ssh/sshd (Only required for cluster installation)
- sshpass
- tar


## Installation

Non-interactive installation uses TigerGraph defaults for all prompts.

### Single Node
Step 1: Extract the package
Extract the package by running the following command to create a folder named tigergraph-<version>-offline.

```bash
   tar -xzf tigergraph-<version>.tar.gz
```
Step 2: Configure installation settings
Navigate to the tigergraph-<version>-offline folder. Inside the folder, there is a file named install_conf.json. For non-interactive mode installation, the user must review and modify all the settings in the file install_conf.json before running the installer.

Reference `install_conf.json` file:

```bash
{
  "BasicConfig": {
    "TigerGraph": {
      "Username": "tigergraph",
      "Password": "tigergraph",
      "SSHPort": 22,
      "PrivateKeyFile": "",
      "PublicKeyFile": ""
    },
    "RootDir": {
      "AppRoot": "/home/tigergraph/tigergraph/app",
      "DataRoot": "/home/tigergraph/tigergraph/data",
      "LogRoot": "/home/tigergraph/tigergraph/log",
      "TempRoot": "/home/tigergraph/tigergraph/tmp"
    },
    "License": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJJc3N1ZXIiOiJUaWdlckdyYXBoIEluYy4iLCJBdWRpZW5jZSI6IlRpZ2VyR3JhcGggRnJlZSIsIlN0YXJ0VGltZSI6MTYxNzM1NTgwMywiRW5kVGltZSI6MTY1MTQ4NzQwMywiSXNzdWVUaW1lIjoxNjE3MzU5NDAzLCJFZGl0aW9uIjoiRW50ZXJwcmlzZSIsIlZlcnNpb24iOiJBbGwiLCJIb3N0Ijp7Ik1heENQVUNvcmUiOjEwMDAwMDAwMDAwMDAwMDAsIk1heFBoeXNpY2FsTWVtb3J5Qnl0ZXMiOjEwMDAwMDAwMDAwMDAwMDAsIk1heENsdXN0ZXJOb2RlTnVtYmVyIjoxMDI0fSwiVG9wb2xvZ3kiOnsiTWF4VmVydGV4TnVtYmVyIjoxMDAwMDAwMDAwMDAwMDAwLCJNYXhFZGdlTnVtYmVyIjoxMDAwMDAwMDAwMDAwMDAwLCJNYXhHcmFwaE51bWJlciI6MTAyNCwiTWF4VG9wb2xvZ3lCeXRlcyI6NTM2ODcwNjM3MTJ9LCJHU1QiOnsiRW5hYmxlIjp0cnVlLCJab29tQ2hhcnRzTGljZW5zZSI6IntcbiAgXCJsaWNlbnNlXCI6IFwiWkNCLTRtZmk3NDRsdjogWm9vbUNoYXJ0cyBFbnRlcnByaXNlIGxpY2VuY2UgZm9yIFRpZ2VyR3JhcGggZm9yIG9mZmxpbmUgdXNlOyB1cGdyYWRlcyB1bnRpbDogMjAyMi0xMi0zMVwiLFxuICBcImxpY2Vuc2VLZXlcIjogXCI4Zjg2ZWQzM2Y0ZWJmYmE4YTI4NjlkZGUzMmYzYTMwZGI4NGEzOTgxYmEzNmZhZWZlYTMxNDhhYzY4MzkxZTlhYzUzZDU3YmI5MjdmNjY5YWI1ZWJhYzJhYmQ3YTFkNDBiM2UxNDRmZjIwMDYyZWNiZmIwZjJiN2I0ZWFmZWIwYzU2NTc2NjBhZGExZDc1MWNhODU3NWZhYTE1ZWQwODI0NzkwZWQxMjJkY2Q4NjcyZTJiN2QzN2MwNmE3MzFhYTc2MDIwM2FhYmMwYjYzOWEzMjBhOGQxNmI0YmFiNGY5NTJiNTMwOTUzMWI4MDkxNjYwZDVjOGMzNGY0NmMyNjZiOGZiNzc2YzFmN2Y0MTdlMGQ5Y2JkZGFlOTExNTFlY2Y3YmMzZDlkNDgyNWE2MjAwYzk0MWMyMDE4ZDY4YjkyOWE5ZWY2MzQ2MDE5NjFhYmU1MGI0ZTk0ZmY5Y2VjMjA1ODEwYmVlZmRkM2NlZDU2YjM5NTVjYmE0YWIyMGNiNzc5MWE0NmQxNzIwMzNiZmI0ZDIyMDM4ODZhZTllZDFkOWMzOWIyOTM2ODc3Yjc2NzY4ZjQwNWQ5Y2MwY2JlODVjOTE2NDllMDI5YTA0NDFlOGFmYjA2MzY4MTMxZGM1YTc1NDEyOTc1NjFlMDRlMGM1MzE1ZmFjMDdhYzViOWViM1wiXG59In0sIlJ1bnRpbWVNZW1vcnkiOnsiTWF4VXNlclJlc2lkZW50U2V0Qnl0ZXMiOjEwMDAwMDAwMDAwMDAwMDB9fQ.B1tR-ZyzFB3WCtCXBl-CXb-3YlL1Hy1btCzsEnkLd6GE2AOvJdpqVZGU-YGyIaOGSYX1sbjrePBoupuWPwrOgvce-mq_Qwp8eounoEOkuzYlTQXFj3lM1wO6vrdiOn2GUm0qMVtlVTIDrFZlZ-bWcdSUA4J8t2JNJrYxQgPWjlO9f4I4w2RbTK3sZW7N96bqFUQPituiwLcPX_VSVT8KBluM2WIH7CJitHl21IbnQbwScBw_QgjfaITZXE6UXisMM9XNphf5yQX9arFDQLchV7e2i3R2tUEuF7V_mHFrVa8vCBjm_0uABzcY8U02QJ78SB9MgLNrqgtzIwHmYP22KQ",
    "NodeList": [
      "m1: 127.0.0.1"
    ]
  },
  "AdvancedConfig": {
    "ClusterConfig": {
      "LoginConfig": {
        "SudoUser": "sudoUserName",
        "Method": "P[or K]",
        "P": "<sudo_user_password>",
        "K": "</path/to/my_key.pem_rsa>"
      },
      "ReplicationFactor": 1
    }
  }
}

```
### Cluster

A highly available cluster has a cluster size of at least 4 and a replication factor of at least 2. A highly available cluster can withstand the failure of any node in the cluster.

Step 1: Extract the package
Extract the package by running the following command to create a folder named tigergraph-<version>-offline.

```bash
   tar -xzf tigergraph-<version>.tar.gz
```
Step 2: Configure installation settings
Navigate to the tigergraph-<version>-offline folder. Inside the folder, there is a file named install_conf.json. For non-interactive mode installation, the user must review and modify all the settings in the file install_conf.json before running the installer.

Reference `install_conf.json` file:

```bash
{
  "BasicConfig": {
    "TigerGraph": {
      "Username": "tigergraph",
      "Password": "tigergraph",
      "SSHPort": 22,
      "PrivateKeyFile": "",
      "PublicKeyFile": ""
    },
    "RootDir": {
      "AppRoot": "/home/tigergraph/tigergraph/app",
      "DataRoot": "/home/tigergraph/tigergraph/data",
      "LogRoot": "/home/tigergraph/tigergraph/log",
      "TempRoot": "/home/tigergraph/tigergraph/tmp"
    },
    "License": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJJc3N1ZXIiOiJUaWdlckdyYXBoIEluYy4iLCJBdWRpZW5jZSI6IlRpZ2VyR3JhcGggRnJlZSIsIlN0YXJ0VGltZSI6MTYxNzM1NTgwMywiRW5kVGltZSI6MTY1MTQ4NzQwMywiSXNzdWVUaW1lIjoxNjE3MzU5NDAzLCJFZGl0aW9uIjoiRW50ZXJwcmlzZSIsIlZlcnNpb24iOiJBbGwiLCJIb3N0Ijp7Ik1heENQVUNvcmUiOjEwMDAwMDAwMDAwMDAwMDAsIk1heFBoeXNpY2FsTWVtb3J5Qnl0ZXMiOjEwMDAwMDAwMDAwMDAwMDAsIk1heENsdXN0ZXJOb2RlTnVtYmVyIjoxMDI0fSwiVG9wb2xvZ3kiOnsiTWF4VmVydGV4TnVtYmVyIjoxMDAwMDAwMDAwMDAwMDAwLCJNYXhFZGdlTnVtYmVyIjoxMDAwMDAwMDAwMDAwMDAwLCJNYXhHcmFwaE51bWJlciI6MTAyNCwiTWF4VG9wb2xvZ3lCeXRlcyI6NTM2ODcwNjM3MTJ9LCJHU1QiOnsiRW5hYmxlIjp0cnVlLCJab29tQ2hhcnRzTGljZW5zZSI6IntcbiAgXCJsaWNlbnNlXCI6IFwiWkNCLTRtZmk3NDRsdjogWm9vbUNoYXJ0cyBFbnRlcnByaXNlIGxpY2VuY2UgZm9yIFRpZ2VyR3JhcGggZm9yIG9mZmxpbmUgdXNlOyB1cGdyYWRlcyB1bnRpbDogMjAyMi0xMi0zMVwiLFxuICBcImxpY2Vuc2VLZXlcIjogXCI4Zjg2ZWQzM2Y0ZWJmYmE4YTI4NjlkZGUzMmYzYTMwZGI4NGEzOTgxYmEzNmZhZWZlYTMxNDhhYzY4MzkxZTlhYzUzZDU3YmI5MjdmNjY5YWI1ZWJhYzJhYmQ3YTFkNDBiM2UxNDRmZjIwMDYyZWNiZmIwZjJiN2I0ZWFmZWIwYzU2NTc2NjBhZGExZDc1MWNhODU3NWZhYTE1ZWQwODI0NzkwZWQxMjJkY2Q4NjcyZTJiN2QzN2MwNmE3MzFhYTc2MDIwM2FhYmMwYjYzOWEzMjBhOGQxNmI0YmFiNGY5NTJiNTMwOTUzMWI4MDkxNjYwZDVjOGMzNGY0NmMyNjZiOGZiNzc2YzFmN2Y0MTdlMGQ5Y2JkZGFlOTExNTFlY2Y3YmMzZDlkNDgyNWE2MjAwYzk0MWMyMDE4ZDY4YjkyOWE5ZWY2MzQ2MDE5NjFhYmU1MGI0ZTk0ZmY5Y2VjMjA1ODEwYmVlZmRkM2NlZDU2YjM5NTVjYmE0YWIyMGNiNzc5MWE0NmQxNzIwMzNiZmI0ZDIyMDM4ODZhZTllZDFkOWMzOWIyOTM2ODc3Yjc2NzY4ZjQwNWQ5Y2MwY2JlODVjOTE2NDllMDI5YTA0NDFlOGFmYjA2MzY4MTMxZGM1YTc1NDEyOTc1NjFlMDRlMGM1MzE1ZmFjMDdhYzViOWViM1wiXG59In0sIlJ1bnRpbWVNZW1vcnkiOnsiTWF4VXNlclJlc2lkZW50U2V0Qnl0ZXMiOjEwMDAwMDAwMDAwMDAwMDB9fQ.B1tR-ZyzFB3WCtCXBl-CXb-3YlL1Hy1btCzsEnkLd6GE2AOvJdpqVZGU-YGyIaOGSYX1sbjrePBoupuWPwrOgvce-mq_Qwp8eounoEOkuzYlTQXFj3lM1wO6vrdiOn2GUm0qMVtlVTIDrFZlZ-bWcdSUA4J8t2JNJrYxQgPWjlO9f4I4w2RbTK3sZW7N96bqFUQPituiwLcPX_VSVT8KBluM2WIH7CJitHl21IbnQbwScBw_QgjfaITZXE6UXisMM9XNphf5yQX9arFDQLchV7e2i3R2tUEuF7V_mHFrVa8vCBjm_0uABzcY8U02QJ78SB9MgLNrqgtzIwHmYP22KQ",
    "NodeList": [
      "m1: 127.0.0.1"
    ]
  },
  "AdvancedConfig": {
    "ClusterConfig": {
      "LoginConfig": {
        "SudoUser": "sudoUserName",
        "Method": "P[or K]",
        "P": "<sudo_user_password>",
        "K": "</path/to/my_key.pem_rsa>"
      },
      "ReplicationFactor": 1
    }
  }
}

[More Reading...](https://docs.tigergraph.com/tigergraph-server/current/installation/install#_installation/)



## System Interaction

- **GSQL client**: One TigerGraph instance can support multiple GSQL clients, on remote nodes.

- **GraphStudio**: Our intuitive graphical user interface, which provides most of the basic GSQL functionality.

- **REST API**: Enterprise applications which need to run the same queries many times can maximize their efficiency by communicating directly with RESTPP.

- **gAdmin** is used for system administration.


## Acknowledgements

 - [Reference Doc](https://docs.tigergraph.com/tigergraph-server/current/intro/)
 
