## [사무용품 견적서 메일발송]  ##
> 견적서 발급 요청 메일의 첨부파일에 적힌 물품의 견적서를 작성하여 메일보내기   
> REFramework로 구현 (QueueItem이 아닌 **DataTable.DataRow** 이용)   
> 견적서 발급 요청 메일이 하나 이상임
> **TransactionData = 메일리스트의 [mailIndex], [mailSubject], [mailAttachmentName], [mailRequestDate]**


#### [작업순서] ####
> _이해를 돕기위한 작업가이드.xlsx 파일 참고
1. 아웃룩 메일 가져오기 (TransactionData에 입력 - [mailIndex], [mailSubject], [mailAttachmentName], [mailRequestDate])
2. 사무용품 사이트(오피스디포) 접속
3. 작업지시서 읽어서 물품코드 검색, 물품의 수량을 작업지시서 대로 입력, 장바구니에 담기
4. 장바구니 페이지에서 견적서 PDF로 저장
5. 견적서 첨부하여 메일 발송. 원본 메일 폴더 이동


#### [Config.xlsx 참고] ####
* strURL - 오피스디포 사이트 URL
* strOutlookAccount - 아웃룩 메일 계정
* strOutlookFolder - 아웃룩 메일가져올 폴더 
* strOutlookMoveFolder - 아웃룩 메일 폴더 - 처리 완료된 메일 이동 폴더
* strToMailAccount - 견적서 받을 메일주소
* strMailSaveFolder - 작업지시서 다운로드 경로
* strMailSubject - 견적서 보낼 메일 제목
* strMailBody - 견적서 보낼 메일 내용
* strPDFFileName - 견적서 pdf 파일명
* strSaveFolderPath - 파일 저장경로

  
#### [추가된 파일(Invoke) 정보] ####
* Framework\CreateFolderFile.xaml - 폴더, 파일 처리. 폴더 있으면 삭제 후 생성, 없으면 생성. 파일 있으면 삭제
* Framework\GetDateFromString.xaml - 문자열에서 날짜부분 가져오기 (20231121_GetDateFromString.xaml 에서 20231121만 추출)


#### [How It Works] ####

1. **INITIALIZE PROCESS**
   + Config.xlsx 세팅
   + 브라우저 강제 종료
   + 아웃룩 메일 읽기 (제목 "견적서", 첨부파일 있음 으로 필터)
      1. 견적서요청 메일 정보 TransactionData에 저장
      2. 견적서 요청(작업지시서) 엑셀파일 저장
   + 오피스디포 브라우저 오픈

2. **GET TRANSACTION DATA**
   + Get transaction item 비활성
   + TransactionNumber가 DataTable Row 수만큼 반복을 위한 처리 -  TransactionData의 Row 갯수 체크해서 TransactionItem에 현재 Row 정보 할당

4. **PROCESS TRANSACTION**
   + 저장된 작업지시서.xlsx 파일 읽기. DataTable에 저장
   + DataTable 반복하면서
     1. 오피스디포 사이트에서 물품코드 검색
     2. 상세페이지에서 수량입력, 장바구니에 담기 클릭
   + 장바구니 클릭, 상품 전체선택 후 pdf 저장. 장바구니 비우기
   + pdf 첨부하여 메일발송. 견적서 발송 완료 메일 폴더 이동

4. **END PROCESS**
   + 브라우저 닫기
   + 브라우저 강제 종료

* * *
![estimate_for_Office_Goods_guide](https://github.com/pnmGithub/estimate_for_Office_Goods.RPA-uipath/assets/149296871/f0721054-4ca0-4ca0-a80e-39d6174e0e50)
* * *

### REFrameWork Template ###
**Robotic Enterprise Framework**
