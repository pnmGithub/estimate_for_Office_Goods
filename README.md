### [사무용품 견적서 메일발송]  ###
> 견적서 발급 요청 메일의 첨부파일에 적힌 물품의 견적서를 작성하여 메일보내기
> REFramework로 구현 (QueueItem이 아닌 DataTable 이용)
> 견적서 발급 요청 메일이 하나 이상임

#### [작업순서] ####
1. 아웃룩 메일 가져오기 (TransactionData에 입력 - [mailIndex], [mailSubject], [mailAttachmentName], [mailRequestDate])
2. 사무용품 사이트(오피스디포) 접속
3. 작업지시서 읽어서 물품코드 검색, 물품의 수량을 작업지시서 대로 입력, 장바구니에 담기
4. 장바구니 페이지에서 견적서 PDF로 저장
5. 견적서 첨부하여 메일 발송. 원본 메일 폴더 이동

#### [Config.xlsx 참고] ####
strURL - 오피스디포 사이트
strOutlookAccount - 아웃룩 메일 계정(보내는 메일계정)
strOutlookFolder - 아웃룩 메일 폴더 
strOutlookMoveFolder - 아웃룩 메일 폴더 - 처리 완료된 메일 이동 폴더
strToMailAccount - 받는 메일주소
strMailSaveFolder - 작업지시서 다운로드 경로
strMailSubject - 견적서 메일 제목
strMailBody - 메일 내용
strPDFFileName - 견적서 pdf 파일명
strSaveFolderPath - 파일 저장경로



### REFrameWork Template ###
**Robotic Enterprise Framework**

* Built on top of *Transactional Business Process* template
* Uses *State Machine* layout for the phases of automation project
* Offers high level logging, exception handling and recovery
* Keeps external settings in *Config.xlsx* file and Orchestrator assets
* Pulls credentials from Orchestrator assets and *Windows Credential Manager*
* Gets transaction data from Orchestrator queue and updates back status
* Takes screenshots in case of system exceptions


### How It Works ###

1. **INITIALIZE PROCESS**
 + ./Framework/*InitiAllSettings* - Load configuration data from Config.xlsx file and from assets
 + ./Framework/*GetAppCredential* - Retrieve credentials from Orchestrator assets or local Windows Credential Manager
 + ./Framework/*InitiAllApplications* - Open and login to applications used throughout the process

2. **GET TRANSACTION DATA**
 + ./Framework/*GetTransactionData* - Fetches transactions from an Orchestrator queue defined by Config("OrchestratorQueueName") or any other configured data source

3. **PROCESS TRANSACTION**
 + *Process* - Process trasaction and invoke other workflows related to the process being automated 
 + ./Framework/*SetTransactionStatus* - Updates the status of the processed transaction (Orchestrator transactions by default): Success, Business Rule Exception or System Exception

4. **END PROCESS**
 + ./Framework/*CloseAllApplications* - Logs out and closes applications used throughout the process


### For New Project ###

1. Check the Config.xlsx file and add/customize any required fields and values
2. Implement InitiAllApplications.xaml and CloseAllApplicatoins.xaml workflows, linking them in the Config.xlsx fields
3. Implement GetTransactionData.xaml and SetTransactionStatus.xaml according to the transaction type being used (Orchestrator queues by default)
4. Implement Process.xaml workflow and invoke other workflows related to the process being automated
