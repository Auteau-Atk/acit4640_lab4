# acit4640_lab4

Create Key Gen:
```bash
ssh-keygen -t rsa -C "clout-init" -f ~/.ssh/cloud-init
```

Import to aws:
```bash
aws ec2 import-key-pair --key-name "cloud-init" --public-key-material fileb://~/.ssh/cloud-init.pub
```

Contents of the public key will go into the /scripts/cloud-config.yaml

This will also include the userdata to download nginx and nmap
```yaml
#cloud-config
users:
  - name: web
    primary_group: web
    groups: wheel
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - public key contents
packages:
  - nginx
  - nmap

runcmd:
  - systemctl enable --now nginx
```


While in repository directory:
```bash
terraform init
terraform fmt
terraform validate
terraform plan -out cloud
terraform apply "cloud"
```

After the terraform script is done, it will print out the public dns and ip

SSH to vm
```bash
ssh -i ~/.ssh/cloud-init web@<public ip>
```

Verify Nginx is downloaded and nmap
```bash
sudo systemctl status nginx
nmap --version
```