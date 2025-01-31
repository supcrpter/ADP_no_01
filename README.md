# ADP_no_01

nillion

1. **确保Keplr钱包拥有NIL**

您需要先确保Keplr钱包中有NIL代币。可以使用Nillion水龙头获取NIL。请按照以下链接访问Nillion水龙头以获得代币：

- https://faucet.testnet.nillion.com/

2. **安装Docker**

[VPS安装准备](https://www.notion.so/VPS-11f3d17476d1806bb7dfcc8259f67778?pvs=21) 

Verifier依赖于Docker，因此首先要安装Docker。在您的VPS上运行以下命令来安装Docker：

```bash
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install docker-ce
```

安装完成后，启动并设置Docker服务为开机自动启动：

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

3. **验证Docker安装**

您可以通过运行以下命令来验证Docker是否已成功安装：

```bash
docker --version
```

正常输出应类似于：Docker version 27.1.1, build 63125853e3

运行以下命令来确保Docker能正常工作：

```bash
docker container run --rm hello-world
```

如果一切正常，您将看到 "Hello from Docker!" 的输出。

4. **获取Verifier Docker镜像**

通过Docker Hub拉取Nillion Verifier的镜像：

```bash
docker pull nillion/verifier:v1.0.1
```

5. **初始化Verifier**

首先创建一个目录来存储Verifier的数据：

```bash
mkdir -p ~/nillion/verifier
```

然后初始化Verifier：

```bash
docker run -v ~/nillion/verifier:/var/tmp nillion/verifier:v1.0.1 initialise #不是跑程序，这是生成钱包
```

这将输出Verifier的注册信息，您需要保存这些信息，包括：

- Verifier账户ID
- Verifier公钥
- 完整的Verifier连接链接

6. **为Verifier提供资金**

要在Nillion链上挑战秘密，Verifier账户需要NIL资金。您可以使用Nillion水龙头为Verifier账户充值：

- https://faucet.testnet.nillion.com/

7. **运行Verifier**

使用nohup（关闭终端后继续运行）命令启动Verifier：
```bash
nohup docker run -v ./nillion/verifier:/var/tmp nillion/verifier:v1.0.1 verify --rpc-endpoint "https://testnet-nillion-rpc.lavenderfive.com" > output.log 2>&1 &
```
