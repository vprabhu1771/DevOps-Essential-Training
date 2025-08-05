Deploy React App to AWS EC2

```
https://www.youtube.com/watch?v=0gJQspNUaMM
```

AWS -> Login -> Console Home -> EC2 -> Launch instance


# Launch an instance

Name and tags

Name -> My Web Server


Application and OS Images (Amazon Machine Image)

Quick Start -> Ubuntu


Instance type

Instance type -> t2.micro Free tier eligible


Key pair (login)

Key pair name - required -> Create new key pair


# Create key pair

Key pair name -> some_name

Key pair type -> RSA

Private key file format -> pem


# Instances -> Connect -> SSH client -> Copy Paste Example ssh into Git Bash

sudo apt update

sudo apt install git

sudo apt install -y nodejs npm

# Clone React Project

git clone URL


Create key pair
