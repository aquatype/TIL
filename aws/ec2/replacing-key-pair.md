# Replacing key pair

EC2 인스턴스 생성 시 발급받았던 키가 분실되었을 때 이를 교체한 방법을 기록한다.  
아마존에서는 [AMI를 이용하는 방법]을 권장하고 있지만, 기존 인스턴스의 스냅샷을 떠서 그걸로 새로 인스턴스를 만들어야 하고 설정을 고스란히 붙여줘야 하고 *아무튼 귀찮다*.
그냥 볼륨만 떼어내서 다른 인스턴스에 붙인 다음 키를 갈아치우는 방법을 시도해봤는데, 그나마 조금 덜 귀찮은 것 같다.

1. 해당 인스턴스를 중지하고, 메인 볼륨을 detach한다.
2. 새롭게 생성한 인스턴스, 또는 접근 권한이 있는 기존 인스턴스에 해당 볼륨을 `/dev/xvdf`로 attach한다.
3. 해당 인스턴스 터미널 접속 후 해당 볼륨에 인증 키를 복사한다.
```
$ mkdir /mnt/tmp; sudo mount /dev/xvdf1 /mnt/tmp
$ cp ~/.ssh/authorized_keys /mnt/tmp/home/ubuntu/.ssh/authorized_keys
$ umount /mnt/tmp
```
4. 볼륨을 detach한 다음 원래 인스턴스에 `/dev/sda1`로 attach한다.

여전히 EC2 콘솔에서 표시되는 키는 기존 그대로이지만, 새로운 키로 접속이 가능해진다.


[AMI를 이용하는 방법]:https://aws.amazon.com/ko/premiumsupport/knowledge-center/ec2-windows-replace-lost-key-pair/
