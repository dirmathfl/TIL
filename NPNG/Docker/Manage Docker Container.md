#  Manage Docker Container

> Install & Run 과정에서 도커 컨테이너를 실행하기 위한 전반적인 이해를 할 수 있었다. 이는 도커 컨테이너를 실행하기 위한 개요를 이해한 것 뿐이다. 따라서 볼륨, 네트워크, 자원 할당와 같이 데이터 보장과 네트워크 구성, 자원 관리에 대해 이해한다면 효율적으로 컨테이너를 활용할 수 있을 것이다. :slightly_smiling_face:



# Volume

 도커 이미지를 통해 컨테이너를 생성하게 되면, 기존의 이미지는 읽기 전용 상태이며 변경이 불가능하다. 따라서 컨테이너의 변동된 정보만 별도로 저장하여 각 컨테이너 정보를 유지한다. 컨테이너를 삭제하게 되면 컨테이너 정보는 삭제되게 되며, 복구 할 수 있는 방법이 없다. 이에 데이터를 보존하기 위해 사용하는 방식이 **도커 볼륨**이다.



## 1. 호스트 볼륨 공유

 `docker run -v host_dir:container_dir`와 같이 `-v` 옵션을 통해 호스트의  특정 디렉토리에 컨테이너의 특정 디렉토리를 공유할 수 있다. 따라서 `-v` 옵션을 통해 공유된 컨테이너 디렉토리는 호스트 디렉토리에 저장되게 되며, 이는 해당 컨테이너가 삭제되더라도 정보를 보존할 수 있도록 한다.

 이는 호스트에 위치한 디렉토리를 컨테이너에 마운트 하게 됨으로써 가능하다. 따라서 동일한 위치를 다른 컨테이너에서 `-v`옵션을 통해 지정하게 될 경우, 해당 위치의 원래 데이터들은 덮어쓰기가 되므로 주의가 필요하다.



## 2. 볼륨 컨테이너

 `-v` 옵션을 통해 생성된 컨테이너, 즉 호스트의 디렉토리를 마운트하여 데이터를 공유하는 컨테이너를 **볼륨 컨테이너**의 역할을 수행 하기도 한다. 이는 호스트 볼륨을 공유하는 기능 이외의  다른 기능을 수행하지 않는 컨테이너를 의미한다.

 `docker run --volumes-from container_name`와 같이 `--volumes from`이라는 옵션을 통해 공유 받을 컨테이너를 지정할 수 있다.

 

## 3. 도커 볼륨

 앞의 방식들은 호스트의 볼륨을 공유하는 방법으로 데이터를 보존 할 수 있다. 이 방법 외에도 도커 자체에서 제공하는 볼륨을 통해 데이터를 보존할 수 있다.



### 가. 도커 볼륨 생성

 ```shell
$ docker volume create --name test
test	
 ```

 위와 같이 `docker volume create`를 통해 볼륨을 생성 할 수 있으며, `ls`를 통해 이를 확인할 수 있다.  기존의 호스트 볼륨을 공유하는 방식의 차이점은 볼륨 관리를 도커에서 하게 되므로, 사용자는 볼륨의 위치가 호스트 내에 어디에 위치하는지에 대한 정보를 제공하는 것이 아닌, 볼륨의 이름을 사용하는 것으로 편리하게 사용할 수 있다.



```shell
$ docker run -i -t --name volume_test \
-v test:/root/ \
ubuntu:16.04
```

 위와 같이 `-v` 옵션을 통해 `docker volume:container_dir`을 지정하여 root 디렉토리 하위 내역을 공유하게 되므로 해당 볼륨을 통해 데이터를 저장하거나 공유할 수 있다.



```shell
$ docker run -i -t --name auto_test \
-v /root \
ubuntu:16.04
```

 위와 같이 사용할 볼륨 이름을 입력하지 않는 다면, 자동으로 사용할 수 있는 볼륨을 생성하게 된다.



### 나. 도커 볼륨 목록 및 정보 확인

```shell
$ docker volume ls
DRIVER              VOLUME NAME
local               test
```

`docker volume ls`를 통해 현재 생성된 volume에 대한 정보를 확인 할 수 있다. 



```shell
$ docker volume inspect test 
[
    {
        "CreatedAt": "2020-07-14T00:04:30+09:00",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/test/_data",
        "Name": "test",
        "Options": {},
        "Scope": "local"
    }
]
```

 실제 저장된 위치를 알고 싶다면 `docker volume inspect`를 통해 확인 가능하다.



```shell
        "Mounts": [
            {
                "Type": "volume",
                "Name": "test",
                "Source": "/var/lib/docker/volumes/test/_data",
                "Destination": "/root",
                "Driver": "local",
                "Mode": "z",
                "RW": true,
                "Propagation": ""
            }
        ],
```

 해당 볼륨을 사용하는 컨테이너를 확인하기 위해 `docker container inspect container_name`을 통해 확인가능하다. 이와 같이 도커는 모든 명령어에 `container`, `image`, `volume`과 같이 명시하여 특정 구성 단위를 제어 할 수 있다는 것을 알 수 있다.



### 다. 도커 볼륨 삭제

```shell
$ docker volume rm test
test
```

 컨테이너를 삭제 할 때와 마찬가지로 `rm`을 통해 삭제 가능하며 `prune`을 통해 모든 볼륨을 삭제 할 수 도 있다.



# Network

 앞서 컨테이너를 생성할 때 도커 컨테이너를 외부에서 접속하기 위한 방법으로 바인딩이 있다고 간략하게 다루었다. 도커 컨테이너 내에서 네트워크 정보를 확인하기 위해서 `ifconfig` 명령어를 사용하면 다음과 같이 네트워크 구성에 대해 확인 할 수 있다. 

 만약 `ifconfig`의 명령어가 실행되지 않는다면 `apt-get update && apt-get install net-tools` 명령을 통해 `net-tools` 설치를 진행하여야 한다.



## 1. 도커 네트워크 구조

```shell
$ ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:ac:11:00:02  
          inet addr:172.17.0.2  Bcast:172.17.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:8618 errors:0 dropped:0 overruns:0 frame:0
          TX packets:5832 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:17177039 (17.1 MB)  TX bytes:319563 (319.5 KB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

```

 여기서 `eth0`는 컨테이너의 네트워크 인터페이스를 의미하며, `lo`는 호스트의 네트워크 인터페이스를 의미한다.



```shell
$ ifconfig
docker0   Link encap:Ethernet  HWaddr 02:42:a9:72:37:e9  
          inet addr:172.17.0.1  Bcast:172.17.255.255  Mask:255.255.0.0
          inet6 addr: fe80::42:a9ff:fe72:37e9/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:6601 errors:0 dropped:0 overruns:0 frame:0
          TX packets:9777 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:270068 (270.0 KB)  TX bytes:19144139 (19.1 MB)


veth918a124 Link encap:Ethernet  HWaddr 0e:5b:ee:e3:64:41  
          inet6 addr: fe80::c5b:eeff:fee3:6441/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:5832 errors:0 dropped:0 overruns:0 frame:0
          TX packets:8618 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:319563 (319.5 KB)  TX bytes:17177039 (17.1 MB)
```

 도커 컨테이너 생성 후에 호스트에서 `ifconfig`를 입력하면 `docker0`와  `veth` 네트워크 인터페이스가 생성된 것을 확인할 수 있다.



### 가. veth 인터페이스

 호스트 상에 생성된 `veth` 네트워크 인터페이스는 virtual ethernet을 의미하며, 하나의 veth는 컨테이너의 eth0로 연결된다.  따라서 `veth`는 컨테이너의 수 만큼 호스트 OS에 생성되게 된다.



### 나. docker0 인터페이스

 `docker0` 네트워크 인터페이스는 모든 veth를 호스트의 eth와 연결시켜주는 브리지 역할을 한다. 즉 컨테이너의 네트워크 구조는 다음과 같다 `컨테이너 eth →  호스트 vth → docker0 → 호스트 eth`를 통해 접근할 수 있는 구조를 가지고 있어 바인딩을 통해 외부에서도 컨테이너에 접근 할 수 있다.



## 2. 도커 네트워크 기능

 기본 도커의 네트워크 구조에서 사용자의 설정에 따라, 브리지, 호스트를 변경하여 사용할 수 있다. 이는 `docker network`의 다양한 명령어를 통해 가능하다.



### 가. 브리지 네트워크

 기본적으로 사용되는 브리지 네트워크는 `docker0`이지만 사용자 정의에 따라 브리지 네트워크를 변경할 수 있다.

```shell
$ docker network create --driver bridge bridge_test
```

```shell
$ docker run -i -t --name bridget_container \
--net bridget_test \
ubuntu:16.04
```

```shell
$ docker network disconnect bidget_test bridget_container
$ docker network connect bidget_test bridget_container
```

 사용자 지정으로 생성된 네트워크 드라이버는 `connect`, `disconnect`를 통해 네트워크를 연결하거나 끊을 수 있다. 별도의 네트워크 공간을 할당하기 다양한 옵션을 지정할 수 도 있다.



### 나. 호스트 네트워크

```shell
$ docker run -i -t --name host_net_container \
--net host \
ubuntu:16.04
```

 `--net` 옵션에 `host`라고 명시하게 되면, 호스트의 네트워크와 동일한 환경을 컨테이너에 사용할 수 있다. 따라서 컨테이너에 호스트 네트워크를 사용하게 되면 별도의 바인딩 과정 필요 없이 호스트 네트워크에 접근하는 것 만으로 컨테이너에 접근 할 수 있다.



### 다. 논 네트워크

```shell
$ docker run -i -t --name none_net_container \
--net none \
ubuntu:16.04
```

 `--net` 옵션에 `none`이라고 명시하게 되면, 네트워크를 사용하지 않는 컨테이너를 생성할 수 있다. 이는 외부와의 단절을 의미하며 네트워크 인터페이스는 호스트와의 인터페이스인 `lo`만 존재하게 된다.

 

### 라. 컨테이너 네트워크

```shell
$ docker run -i -t --name test_container2 \
--net container:test_container2 \
ubuntu:16.04
```

 `--net` 옵션에 `container:container_name`을 명시하게 되면 해당 컨테이너의 네트워크 인터페이스와 동일한 조건으로 컨테이너를 생성하게 된다. 이와 같이 생성된 컨테이너들은 동일한 `컨테이너 eth → 호스트 veth`를 가지게 됩니다.



# Resource Isolation

 도커는 각 컨테이너의 사용 목적에 따라 시스템의 자원 할당을 제한 할 수 있다. `docker run`의 명령어 뒤에 관련된 시스템 자원에 대한 인자를 통해 설정할 수 있다. **시스템 자원 할당은 도커에서 제공하는 기능이 아닌 리눅스 커널의 Cgroups를 통해 이루어 진다.**



## 1. 메모리 제한

```shell
$ docker run --memory "2g" --name test_container ubuntu:16.04
```

 이와 같이 컨테이너가 사용할 수 있는 메모리를 제한하여 생성한 경우에는, 할당된 메모리를 초과할 경우 컨테이너가 종료되게 된다. 따라서 사용 목적에 맞게 적절하게 메모리를 할당하여야 한다.



## 2. CPU 제한

```shell
$ docker run --cpu-shares 1024 --name test1 ubnutn:16.04
$ docker run --cpu-shares 512 --name test2 ubnutn:16.04
```

 만약 cpu의 부하가 심한 작업을 하는 2개의 컨테이너가 있고, 상대적으로 하나의 컨테이너의 작업이 우선 시 된다면 위와 같은 옵션을 통해 cpu 사용량의 차등을 둘 수 있다. `cpu-shares` 옵션 뒤의 1024, 512와 같은 weight를 통해 상대적으로 얼만큼의 cpu를 사용할지 결정된다. 위의 예시에서 두 개의 컨테이너 모두 cpu 부하량이 높을 경우 test1 컨테이너의 경우 약 69%의 cpu 사용률, test2 컨테이너의 경우 약 33%의 cpu 사용률을 가지게 된다.



 또한 `--cpuset-cpu`를 통해 특정 cpu만 사용하도록 지정 할 수 있으며,  `--cpu-period`, `--cpu-quota`를 통해 스케줄러의 주기를 선택 주기를 변경할 수 도 있다. `--cpus`는 해당 컨테이너가 몇개의 cpu를 사용할 지 지정함으로써 `--cpu-period`, `--cpu-quota`보다 직관적으로 사용할 수 있다.



## 3. Block I/O 제한

```shell
$ docker run -it \
--device-write-bps /dev/nvme0n1:100mb \
ubuntu:16.04
```

 `--device-write-bps`,  `--device-read-bps`,  `--device-write-iops`,   `--device-read-iops`와 같은 옵션을 통해 컨테이너의 Block I/O의 속도를 제한 할 수 있다. 이는 Buffered IO가 아닌 **Direct IO만 해당**한다.



