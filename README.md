# self_issue

在部署fabric网络时，执行./netword.sh down 来down之前的网络部署时，报错如下:
```ERROR: error while removing network: network fabric_test id efa474c66460892ce08d4fb432f6de6b7c6a687d1f6ad38204bafa2ea3f33d5e has active endpoints```
之后的./network.sh createChannel 失败无法创建

```
[➜  test-network git:(main) docker network inspect fabric_test
    {
        "Name": "fabric_test",    # 参数一
        "Id": "efa474c66460892ce08d4fb432f6de6b7c6a687d1f6ad38204bafa2ea3f33d5e",
        "Created": "2022-07-16T11:27:36.753526292Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": true,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "3bf815a1bb3dbfb8d1adf332e4aec67b71c228f39999a209f1e6a27cb53b098e": {
                "Name": "logspout",   # 参数二
                "EndpointID": "71ed41025bd3ea4e84774f2a51df121754d18a05a5adf552f7437364adc61027",
                "MacAddress": "02:42:ac:12:00:06",
                "IPv4Address": "172.18.0.6/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {
            "com.docker.compose.network": "fabric_test",
            "com.docker.compose.project": "compose",
            "com.docker.compose.version": "1.29.2"
        }
    }
]
// 断开网络，这里有俩个参数，分别是参数一参数二在上面标出来了
➜  test-network git:(main) docker network disconnect -f fabric_test logspout
// 然后重新执行，成功down掉，然后重新搭建通道成功
➜  test-network git:(main) ./network.sh down                                
Using docker and docker-compose
Stopping network
Removing network fabric_test
Removing network compose_default
WARNING: Network compose_default not found.
Removing volume compose_orderer.example.com
Removing volume compose_peer0.org1.example.com
Removing volume compose_peer0.org2.example.com
Removing volume compose_peer0.org3.example.com
WARNING: Volume compose_peer0.org3.example.com not found.
Error: No such volume: docker_orderer.example.com
Error: No such volume: docker_peer0.org1.example.com
Error: No such volume: docker_peer0.org2.example.com
Removing remaining containers
Removing generated chaincode docker images
"docker kill" requires at least 1 argument.
See 'docker kill --help'.

Usage:  docker kill [OPTIONS] CONTAINER [CONTAINER...]

Kill one or more running containers
