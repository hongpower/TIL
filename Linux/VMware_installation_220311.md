# vmware 설치

- vmware 웹사이트 접속 :
  - https://www.vmware.com/
- workspaces -> see all products

![image](https://user-images.githubusercontent.com/96896873/158003330-241bf0e8-7775-4210-9b6d-1fbfb7f494f0.png)



- workspace -> Desktop Hypervisor

![image](https://user-images.githubusercontent.com/96896873/158003360-e3274320-4282-41d8-87b1-d99c0f0be2ed.png)



- workstation player 클릭

![image](https://user-images.githubusercontent.com/96896873/158003371-d285a4a3-c871-4096-ad75-477a8b293ef4.png)



- download for free 클릭

![image](https://user-images.githubusercontent.com/96896873/158003380-0f6d3d4e-d0b4-477e-a28c-7f8dc82d3715.png)

- GO TO DOWNLOADS

![image](https://user-images.githubusercontent.com/96896873/158003391-ae8975cb-b577-4c1d-b9ca-10052f25eeb0.png)



- 윈도우면 Download NOW

![image](https://user-images.githubusercontent.com/96896873/158003398-b10eeff4-ed5a-4152-917e-007125041415.png)

- 다운로드된 파일 실행!

![image](https://user-images.githubusercontent.com/96896873/158003402-65431db8-5a73-4120-972a-0e953bdd9565.png)



- custom setup에 둘다 체크 

![image](https://user-images.githubusercontent.com/96896873/158003408-020ad72a-22e0-44c2-b8b0-0c569bbacc65.png)



- User Experience Settingsware부터 끝까지 다 체크된 상태에서 Next 



- 성공적으로 VMware workstation 설치!

![image](https://user-images.githubusercontent.com/96896873/158003414-e2b41cef-f099-4690-a56c-d72ff472dd41.png)



# 가상 머신 만들기

- VMware Workstation 실행
- create a new virtual machine 클릭
- 기존에 ubuntu 다운로드 했다면 : 
  - Installer disc image file 선택

- 

![image](https://user-images.githubusercontent.com/96896873/158003422-3563c801-7d25-4792-8d88-a35e70aa7b7f.png)

- 이미지 우리가 우분투 다운받았던 곳 위치 browse 경로로 지정하고

![image](https://user-images.githubusercontent.com/96896873/158003429-1ed276a4-e00a-42d6-956b-60ebf9ff3375.png)



- Browse 했을 때 image file 잘 잡히는거 확인 가능!

![image-20220311101313898](Images/image-20220311101313898.png)



![image-20220311101447859](Images/image-20220311101447859.png)

- 정보입력
- virtual machine name 지정



- disk capacity지정은 20gb 지정

![image](https://user-images.githubusercontent.com/96896873/158003457-01648aa2-823c-4ba8-9b33-05d03aa30ca5.png)

- single file은 혼자서 사용할 때, 

- split disk는 large disk를 차지하기때문에 본인은 single 선택



- customize hardware 클릭해서 메모리 확인해보면

![image](https://user-images.githubusercontent.com/96896873/158003467-44565fba-4c2f-45a4-95e1-c448f30f273f.png)

- 파란색에 가까울수록 본컴퓨터 느려짐 (가상머신의 속도는 향상); 본인은 4GB 선택

- 프로세서는 본인 2로 유지

- network adapter도 pass

- 후에 soundcard 충돌나면 걍 soundcard remove하기

- Finish 하기



- 열심히 다운중

![image](https://user-images.githubusercontent.com/96896873/158003480-dc488f31-0ff3-4f80-8f9c-40ee05c4cf60.png)



- 완료!

  ![image](https://user-images.githubusercontent.com/96896873/158003489-51b2bd46-ac23-4342-a196-a2423852be3c.png)







