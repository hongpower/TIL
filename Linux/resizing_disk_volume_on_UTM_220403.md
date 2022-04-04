# UTM 하드 디스크 용량 늘리기/변경하기

### 1) qmenu 설치

- 터미널 창에서 : `brew install qmenu`



### 2) qcow2 파일 찾기

- Utm 들어가서 용량을 늘리고 싶은 가상환경 오른쪽 마우스 클릭후 `show in finder` 선택
- 그중 원하는 가상환경의 `.utm` 파일 오른쪽 마우스 클릭후 `show package contents`클릭
- `Images`폴더에서 `.qcow2`로 끝나는 파일 선택하기



### 3) 이미지 크기 늘리기

- 다시 터미널 접속
- `qmenu-img resize`까지 타입하고 방금 선택했던 이미지 파일 터미널로 드래그 하면 자동으로 경로가 입력됨. 여기에다가 본인이 원하는 만큼의 용량 입력
  - 예) `qmenu-img resize {경로} +10G `



### 4) gparted에서 할당해주기

- ubuntu에 들어가서 gparted 열기
- 새로생긴 여유공간 할당해주기



### 5) 현재 상황 확인

> 기존에 컴퓨터를 사용했을 때는 하드디스크가 부족하면 확장할 때마다 그 안의 내용을 삭제하고 크기를 늘리는 수밖에 없었음. 이는 큰 금전적 손실이기 때문에 유닉스에서 도입한 체재가 lvm. 파티션 사이즈를 자유롭게 조정 가능함.

- PE : 물리적 하드 디스크 
  - /dev/hda, /dev/sda 등
- PV : 각각의 파티션을 나눈것임
  - /dev/hda1, dev/hda2
- VG : PV(파티션)을 그룹으로 설정
- LV : 마운트 포인터로 사용할 실질적 파티션임
- Filesystem : 리눅스에서 사용하는 모든 파일시스템 사용가능한 곳


- 현재 내 상태 확인 :
  - df -h
  - sudo vg(volume group)display
  - sudo lv(logical volume)display =>lv사이즈가 vg의 크기에 비해 많이 작다면 lv사이즈를 늘려야 된다는 뜻!



### 6) 파티션 크기 늘리기

- sudo lvresize -l + 100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv

  - `/dev/mapper/ubuntu--vg-ubuntu--lv
      Size of logical volume ubuntu-vg/ubuntu-lv changed from 33.72 GiB (8633 extents) to <67.45 GiB (17266 extents).
      Logical volume ubuntu-vg/ubuntu-lv successfully resized.`

  

### 7) filesystem 사이즈 늘리기

- `resize2fs /dev/mapper/ubuntu --vg-ubuntu--lv`



## 번외 :

- 만약 위 과정이 귀찮다면, 임시방편으로
~~~
    <property>
            <name>yarn.nodemanager.disk-health-checker.max-disk.utilization-per-disk-percentage</name>
            <value>98.5</value>
    </property>
~~~

- 노드 매니저가 건강을 위해서 디폴트로 90%까지만 사용가능하게 돼있는데 늘리기 가능!