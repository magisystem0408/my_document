# 「鯨」をEc2で飼育する時


## amazon-linux2にdockerにインストール
```docker
sudo yum install -y docker
sudo systemctl start docker
sudo usermod -a -G docker ec2-user
sudo systemctl enable docker
```

## amazon-linux2にdocker-composeインストール
```docker
sudo curl -L https://github.com/docker/compose/releases/download/1.28.5/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

## ubuntuに「鯨とタコ」インストール
```shell
sudo apt update -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt update -y
sudo apt -y install docker-ce docker-ce-cli containerd.io
sudo gpasswd -a ubuntu docker

sudo apt  install docker-compose
```