# 실습 3: 엔터프라이즈 랜딩 존 및 마이그레이션

### 예상 소요 시간: 120분

이 실습에서는 Azure 마이그레이션과 인프라, 데이터 및 애플리케이션에 대한 검색, 평가, 온프레미스 리소스 적정 규모 조정 등 모든 사전 마이그레이션 단계가 어떻게 포함되는지 학습하게 됩니다. Azure Migrate는 Azure를 위한 간소화된 마이그레이션, 현대화 및 최적화 서비스를 제공합니다.

## 실습 목표

이 실습에서는 다음 작업을 완료하게 됩니다:

- 작업 1: Azure Copilot이 제공하는 Terraform 코드를 사용하여 랜딩 존 생성
- 작업 2: Hyper-V 호스트를 Migration and modernization에 등록
- 작업 3: Hyper-V에서 Azure Migrate로의 복제 활성화
- 작업 4: 서버 마이그레이션

> **참고:** 이 실습은 워크로드 마이그레이션을 위한 기술 도구에 중점을 두고 있지만, 실제 시나리오에서는 포괄적인 장기 계획이 필요합니다. 랜딩 존에 대한 고려 사항에는 네트워크 트래픽, 접근 제어, 리소스 구성 및 거버넌스가 포함되어야 합니다. CAF Migration 및 Foundation Blueprints는 리소스 관리를 위해 Infrastructure as Code(IaC)를 사용하여 사전 정의된 랜딩 존을 배포하는 데 도움을 줄 수 있습니다. 자세한 내용은 [Azure Landing Zones](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/) 및 [Cloud Adoption Framework Azure Migration landing zone Blueprint sample](https://docs.microsoft.com/azure/governance/blueprints/samples/caf-migrate-landing-zone/)을 참조하십시오.

## 작업 1: Azure Copilot이 제공하는 Terraform 코드를 사용하여 랜딩 존 생성

이 작업에서는 Copilot의 도움을 받아 PLZ를 배포합니다.

1. Lab VM의 파일 탐색기로 이동하여 **Downloads (1)** 폴더에서 다운로드된 zip 파일을 찾으십시오. 파일 이름을 **azure-landing-zone-platform (2)**으로 변경하고, 마우스 오른쪽 버튼으로 클릭하여 **Extract All (3)**을 선택하십시오. **Extract (4)**를 클릭하고 완료될 때까지 기다리십시오.

    ![](./images/E3T1S1.png)

    ![](./images/E3T1S1a.png)

2. Azure Portal로 이동하여 **Microsoft Azure (1)**를 클릭하고 **Subscription (2)**을 선택한 후 **Subscription ID (3)**를 복사하여 메모장에 저장하십시오.

    ![](./images/E3T1S2.png)

    ![](./images/E3T1S2a.png)

3. Azure Portal에서 **Gear (1)** 아이콘을 클릭하고 **Directory ID (2)**를 복사하여 메모장에 저장하십시오.

    ![](./images/E3T1S3.png)

4. GitHub Portal로 이동하십시오. 프로필 아이콘 **(1)**을 클릭하고 **Settings (2)**를 선택한 후, 왼쪽 블레이드에서 아래로 스크롤하여 **Developer settings (3)**를 선택하십시오.

    ![](./images/E3T1S4.png)

    ![](./images/E3T1S4a.png)

5. **Personal access token (1)**을 선택하고 **Token (classi) (2)**를 선택한 후, **Generate ne token (3)**을 클릭하고 **Generate new token (4)**을 선택하십시오.

    ![](./images/E3T1S5.png)

6. **Token name (1)**에 **Migration-PAT**를 입력하고 아래로 스크롤하여 **Generate token (2)**을 클릭하십시오. 팝업이 나타나면 **Generate token (3)**을 클릭하십시오. 이제 PAT Token **(4)**을 복사하여 메모장에 저장하십시오.

    ![](./images/E3T1S6.png)

    ![](./images/E3T1S6a.png)

    ![](./images/E3T1S6b.png)

    ![](./images/E3T1S6c.png)

7. 프로필 아이콘 **(1)**을 클릭하고 **Organizations (2)**를 선택하십시오. 조직명을 복사하여 메모장에 저장하십시오.

    ![](./images/E3T1S7.png)

8. 바탕 화면에서 **Visual Studio Code**를 여십시오.

    ![](./images/E3T1S8.png)

9. **File (1)**을 클릭하고 **Open folder (2)**를 선택하십시오.

    ![](./images/E3T1S9.png)

10. **Downloads (1)** 폴더로 이동하여 **azure-landing-zone-platform (2)** 폴더를 선택하고 **Select folder (3)**를 클릭하십시오. **Yes, I trust the author (4)**를 클릭하십시오.

    ![](./images/E3T1S10.png)

    ![](./images/E3T1S10a.png)

11. **Profile Icon (1)**을 클릭하고 **Sign in to use AI features.. (2)**를 선택하십시오. 메시지가 표시되면 **Continue with FitHub (3)**를 선택하십시오.

    ![](./images/E3T1S11insert.png)
    ![](./images/E3T1S11inserta.png)

12. 브라우저로 리디렉션됩니다. **Continue (1)**를 클릭한 다음 **Authorize Visual-Studio-Code (2)**를 선택하고 마지막으로 **Open (3)**을 클릭하십시오.

    ![](./images/E3T1S12insert.png)
    ![](./images/E3T1S12inserta.png)
    ![](./images/E3T1S12insertb.png)

13. Copilot 채팅에 **setup the MCP server (1)**를 입력하고 Enter를 누르십시오. **Allow in this session (2)**을 클릭하십시오.

    ![](./images/E3T1S11.png)

14. Copilot의 응답 **(1)**을 확인하십시오. **Extensions (2)** 아이콘을 클릭하고 **Azure MCP Server (3)**를 선택한 후 **Switch to Pre-Release Version (4)**을 클릭하십시오. 탭을 닫고 **(5)** **Files (6)** 아이콘을 클릭하십시오.

    ![](./images/E3T1S12.png)

15. Copilot 채팅에 **Review the PLZ code**를 입력하고 Enter를 눌러 응답 **(1)**을 확인하십시오. Copilot이 PLZ 코드에서 필요한 입력값을 검토합니다. yaml 파일에 입력값을 채우도록 요청할 것입니다 **(2)**. 메모장에 복사해 둔 subscription id **(3)**를 업데이트하십시오. **github_personal_access_token** 및 **github_runners_personal_access_token**을 Github PAT 토큰으로 업데이트하고, 메모장에 복사해 둔 **github_organization_name**을 업데이트하며, **apply_approvers**에 <inject key="AzureAdUserEmail"></inject>를 업데이트하십시오 **(4)**.

    ![](./images/E3T1S13.png)

16. Copilot 채팅에 **check for any required packages, if any packages are missing install it**를 입력하고 Enter를 누르십시오. (Copilot이 권한을 요청하는 프롬프트가 나타나면 allow를 클릭하십시오.)

    ![](./images/E3T1S14.png)

17. Copilot 채팅에 **proceed for successfull deployment of the PLZ with this code**를 입력하고 Enter를 누르십시오. **Allow**를 클릭하고 채팅에 나타나는 **Focus Terminal**을 클릭하십시오.

    ![](./images/E3T1S15.png)

18. 채팅을 계속 모니터링하십시오. Azure 자격 증명으로 로그인하라는 메시지가 나타나면 Visual Studio Code를 최소화하면 로그인 창이 표시됩니다. **Work or school account (1)**를 선택하고 **Continue (2)**를 클릭하십시오.

    ![](./images/E3T1S16.png)

19. **Sign into Microsoft Azure** 탭이 표시됩니다. 여기에 자격 증명을 입력하십시오:

       **Email/Username:** <inject key="AzureAdUserEmail"></inject>

       ![](./images/AIM-image1.png)


20. 다음으로 비밀번호를 입력하십시오:

       **Password:** <inject key="AzureAdUserPassword"></inject>

       ![](./images/AIM-image2.png)

21. **Stay Signed in?** 팝업이 표시되면 **No**를 클릭하십시오.

    ![](./images/AIM-image3.png)

22. **Yes**를 선택한 다음, **allow your organinzation to manage your device?**에도 **Yes**를 선택하고 **Done**을 클릭하십시오.

    ![](./images/E3T1S20.png)

    ![](./images/E3T1S20a.png)

    ![](./images/E3T1S20b.png)

23. Visual Studio Code로 돌아가서 배포가 완료될 때까지 채팅 출력을 모니터링하십시오.

    ![](./images/E3T1S21.png)

24. Azure Portal로 이동하십시오. **Microsoft Azure (1)**를 클릭하고 **Resource Group (2)**을 선택하십시오.

    ![](./images/E3T1S22.png)

25. Resource Groups에 3개의 새로운 RG가 배포되었습니다. Copilot이 Terraform 코드를 사용하여 각 RG에 배포된 리소스를 확인할 수 있습니다.

    ![](./images/E3T1S23.png)

    ![](./images/E3T1S23a.png)

    ![](./images/E3T1S23b.png)

    ![](./images/E3T1S23c.png)

#### 작업 요약

이 작업에서는 Copilot의 도움을 받아 PLZ를 배포하였습니다.

## 작업 2: Hyper-V 호스트를 Migration and modernization에 등록

이 작업에서는 Hyper-V 호스트(LabVM)를 Migration and Modernization 서비스에 등록합니다. 이 서비스는 Azure Site Recovery를 기본 마이그레이션 엔진으로 사용합니다. 등록 과정의 일부로 Hyper-V 호스트에 Azure Site Recovery Provider를 배포하게 됩니다.

1. **Azure Migrate** 프로젝트로 돌아가십시오. 글로벌 검색에 **SmartHotelMigration<inject key="DeploymentID" enableCopy="false" /> (1)**을 입력하고 **SmartHotelMigration<inject key="DeploymentID" enableCopy="false" /> (2)**을 선택하십시오.

   ![](./images/E3T2S1.png)

2. **Azure Migrate project** 페이지에서 **Overview (1)**를 선택하고 **Create wave (2)**를 선택하십시오.

   ![](./images/E3T2S2.png)

3. **Create Wave** 페이지에서 아래 정보를 선택하십시오:
   - **Wave name**: **SmartHotelMigrationWave (1)**
   - **Assessment**: **business-modernize.... (2)**
   - **Migration Path**: **PaaS prefered (Recommended) (3)**
   - **Create wave (4)**를 선택하십시오.

    ![](./images/E3T2S3.png)
     > **참고**: Wave Planning 생성에 약 15분이 소요됩니다.

4. **Wave Planning (1)** 탭으로 이동하십시오. 생성된 wave가 표시될 때까지 페이지를 **Refresh (2)** 하면서 기다린 후, **SmartHotelMigrationWave (3)**를 선택하십시오.

    ![](./images/E3T2S4.png )

5. **View details (1)**를 선택하십시오.

    ![](./images/E3T2S5.png)

6. **Start installation (1)**을 선택하십시오.

    ![](./images/E3T2S6.png)

7. Discover 페이지로 이동합니다. 아래 정보를 선택하십시오:
   - **Where do you want to migrate to?**: **Azure VM (1)**
   - **Are your machines virtualized?**: **Yes, with Hyper-V (2)**
   - **Target region**: **West Europe (3)**
   - 체크박스를 반드시 선택하십시오 **(4)**.
   - **Create resource (5)**를 선택하십시오.

    ![](./images/E3T2S7.png)

8. **Download (1)**을 클릭하여 ASR 설치 프로그램을 다운로드하고, **Download (2)**를 클릭하여 Registration Key 파일을 다운로드하십시오.

    ![](./images/E3T2S8.png)

9. labVM 콘솔에서 **Downloads (1)** 폴더로 이동하여 Azure Site Recovery Provider **(2)**를 선택하고 마우스 오른쪽 버튼으로 클릭하여 파일을 **Open (2)** 하십시오.

    ![](./images/E3T2S9.png)

10. Azure Site Recovery Provider Setup Wizard의 Microsoft Update 탭에서 **Off (1)**를 선택하고 **Next (2)**를 클릭하십시오.

    ![](./images/E3T2S10.png)

11. Installation 탭에서 **Install (1)**을 선택하십시오.

    ![](./images/E3T2S11.png)

12. 설치가 완료되면 **Register**를 선택하십시오.

    ![](./images/E3T2S12.png)

13. Vault Setting 탭에서 **Browse**를 클릭하여 키를 가져오십시오. **Downloads (1)** 폴더로 이동하여 키 **(2)**를 선택하고 **Open (3)**을 클릭하십시오.

    ![](./images/E3T2S13.png)

    ![](./images/E3T2S13a.png)

14. Vault Setting 탭에서 **Next**를 선택하십시오.

    ![](./images/E3T2S14.png) 

15. Proxy Setting 탭에서 **(1)**을 선택하고 **Next (2)**를 클릭하십시오.

    ![](./images/E3T2S15.png) 

16. 서버가 성공적으로 등록되면 **Finish**를 클릭하십시오.

    ![](./images/E3T2S16.png)

17. Azure Portal로 돌아가서 탭을 **Refresh (1)** 하고 **Finalize registration (2)**을 클릭하십시오. 등록이 완료되면 **Close (1)**를 클릭하십시오.

    ![](./images/E2T2S17.png)

    ![](./images/E2T2S17a.png)
     > **참고**: Azure Migrate가 Hyper-V 호스트와의 등록을 완료합니다. 등록이 완료될 때까지 기다려 주십시오. 5~10분 정도 소요될 수 있습니다.

     <validation step="2eb6b36f-a031-40f0-8bad-a1748a3c532a" />

#### 작업 요약

이 작업에서는 Hyper-V 호스트를 Azure Migrate Server Migration 서비스에 등록하였습니다.

### 작업 3: Hyper-V에서 Azure Migrate로의 복제 활성화

이 작업에서는 온프레미스 가상 머신의 Hyper-V에서 Azure Migrate Server Migration 서비스로의 복제를 구성하고 활성화합니다.

1. **Azure Migrate Project** 페이지에서 **Overview (1)** 탭을 선택하고 Migration 타일 아래의 **Start execution (3)**을 선택하십시오.

    ![](./images/E3T3S1.png)
   
2. **Specific Intent** 페이지로 이동합니다. 다음 세부 정보를 선택하십시오:

    -  What do you want to migrate? : **Servers or **Virtual machines (VM) (1)**
    -  Where do you want to migrate to? : **Azure VM** **(2)**
    -  How will you select workloads? : **From an assessment (3)**
    -  Discovery method : *SmartHotelAppl (HyperV) (4)**
    -  **Continue (5)**를 클릭하십시오.

       ![](./images/E3T3S2.png)

3.  Execute migration의 Workloads 탭에서 3개의 VM **smarthotelweb1, smarthotelweb2 & UbuntuWAF (1)**를 선택하십시오.

    ![](./images/E3T3S3.png)

4. **Target settings** 탭에서 아래 정보를 선택하십시오:

   - **Subscription**: 구독을 선택하십시오 **(1)**
   - **Resource group**: 기존 **SmartHotelHostRG<inject key="DeploymentID" enableCopy="false" /> (2)**를 선택하십시오.
   - **Cache storage account**: 작업 1에서 생성한 스토리지 계정을 드롭다운에서 선택하십시오 **(3)**.
   - **Virtual network**: **SmartHotelVNet (4)**를 선택하십시오.
   - **Subnet**: **SmartHotel (5)**을 선택하십시오.
   - **Availability option:** **No infrastructure redundancy required (6)**를 선택하십시오.
   - 나머지 값은 기본값으로 두고 **Next (7)**를 선택하십시오.
   
     ![](./images/E3T3S4.png)

     > **참고:** 이 실습에서는 간소화를 위해 마이그레이션된 VM에 대해 고가용성을 구성하지 않습니다. 각 애플리케이션 계층은 단일 VM을 사용하여 구현되기 때문입니다.

5. **Compute** 탭에서 각 가상 머신에 대해 다음 설정을 구성하십시오:

   - 모든 VM의 **Azure VM Size**를 **Standard_F2s_v2**로 선택하십시오 **(1)**.
   - **smarthotelweb1**, **smarthotelweb2** 가상 머신의 **OS Type**은 `Windows`로, **Operating System**은 `Windows Server 2019`로 선택하십시오.
   - **UbuntuWAF** 가상 머신은 **Linux** 운영 체제를 선택하십시오.
   - **Next (2)**를 클릭하여 진행하십시오.

     ![](./images/E3T3S5.png)
    
6. **Disks** 탭에서 설정을 확인하되 변경하지 마십시오. **Next**를 선택한 다음 **Tags** 탭에서 **Next**를 선택하고, 구성 세부 정보를 확인한 후 **Execute Replicate**를 선택하여 서버 복제를 시작하십시오.

    ![](./images/E3T3S6.png)

    ![](./images/E3T3S6a.png)

7. Azure Migrate 프로젝트로 이동하여 **Migrations (1)**를 선택하고 **Refresh (2)**를 클릭하면 Migration Execution 상태 **(3)**를 확인할 수 있습니다.

    ![](./images/E3T3S7.png)
    > **참고**: 복제에 약 10분이 소요됩니다.
    
     <validation step="b38db9a2-1678-47a6-a053-c863b83f8ada" />

#### 작업 요약

이 작업에서는 Hyper-V 호스트에서 Azure Migrate로의 복제를 활성화하고 Azure에서 복제된 VM 크기를 구성하였습니다.



### 작업 4: 서버 마이그레이션

이 작업에서는 UbuntuWAF, smarthotelweb1 및 smarthotelweb2 머신을 Azure로 마이그레이션합니다.

> **참고**: 실제 시나리오에서는 최종 마이그레이션 전에 테스트 마이그레이션을 수행해야 합니다. 시간 절약을 위해 이 실습에서는 테스트 마이그레이션을 건너뜁니다. 테스트 마이그레이션 프로세스는 최종 마이그레이션과 매우 유사합니다.

1. **Refresh (1)**를 클릭하여 페이지를 새로 고치십시오. 3개의 서버에 대해 **Action Pending (3)**이 표시됩니다.

    ![](./images/E3T4S1.png)

2. **smarthotelweb1** VM에서 **Action Pending (1)**을 선택하고 **Testing (2)**을 클릭한 후 **Start test migration (3)**을 선택하십시오.

    ![](./images/E3T4S2.png)

3. 드롭다운에서 가상 네트워크 **SmartHotelVNet (1)**을 선택하고 **Test migration (2)**을 선택하십시오.

    ![](./images/E3T4S3.png)
   
4. **smarthotelweb2** VM에서 **Action Pending (1)**을 선택하고 **Testing (2)**을 클릭한 후 **Start test migration (3)**을 선택하십시오.

    ![](./images/E3T4S4.png)

5. 드롭다운에서 가상 네트워크 **SmartHotelVNet (1)**을 선택하고 **Test migration (2)**을 선택하십시오.

    ![](./images/E3T4S5.png)

6. **UbuntuWAF** VM에서 **Action Pending (1)**을 선택하고 **Testing (2)**을 클릭한 후 **Start test migration (3)**을 선택하십시오.

    ![](./images/E3T4S6.png)

7. 드롭다운에서 가상 네트워크 **SmartHotelVNet (1)**을 선택하고 **Test migration (2)**을 선택하십시오.

    ![](./images/E3T4S7.png)

8. **Refresh (1)**를 클릭하여 페이지를 새로 고치면 Execution status가 **In Progress (2)**로 표시됩니다.

    ![](./images/E3T4S8.png)
       > **참고**: 약 10~15분이 소요됩니다.

9. **Refresh (1)**를 클릭하여 페이지를 새로 고치면 Execution status가 **Actino pending (2)**으로 표시됩니다.

    ![](./images/E3T4S9.png)

10. **smarthotelweb1** VM에서 **Action Pending (1)**을 선택하고 **Testing (2)**을 클릭한 후 **Cleanup test migration (3)**을 선택하십시오. 체크박스 **(4)**를 반드시 선택하고 **Cleanup Test (5)**를 클릭하십시오.

    ![](./images/E3T4S10.png)

    ![](./images/E3T4S10a.png)

11. **smarthotelweb2** VM에서 **Action Pending (1)**을 선택하고 **Testing (2)**을 클릭한 후 **Cleanup test migration (3)**을 선택하십시오. 체크박스 **(4)**를 반드시 선택하고 **Cleanup Test (5)**를 클릭하십시오.

    ![](./images/E3T4S11.png)

    ![](./images/E3T4S11a.png)

12. **UbuntuWAF** VM에서 **Action Pending (1)**을 선택하고 **Testing (2)**을 클릭한 후 **Cleanup test migration (3)**을 선택하십시오. 체크박스 **(4)**를 반드시 선택하고 **Cleanup Test (5)**를 클릭하십시오.

    ![](./images/E3T4S12.png)

    ![](./images/E3T4S12a.png)
       > **참고**: 약 5분이 소요됩니다.

13. **Refresh (1)**를 클릭하면 Execution State가 **Completed (2)**로, Execution status가 **Action pending (3)**으로 표시됩니다.

    ![](./images/E3T4S13.png)

14. **smarthotelweb1** VM에서 **Action Pending (1)**을 선택하고 **Completion (2)**을 클릭한 후 **Migrate (3)**를 선택하십시오. 다음 단계에서 **Shutdown virtual machine and perform a planned migration with no data loss?** 항목에 **Yes (4)**를 선택하고, **Capacity Reservation**에 **None (5)**을 선택한 후 **Migrate (6)**를 클릭하십시오.

    ![](./images/E3T4S14.png)

    ![](./images/E3T4S14a.png)

15. **smarthotelweb2** VM에서 **Action Pending (1)**을 선택하고 **Completion (2)**을 클릭한 후 **Migrate (3)**를 선택하십시오. 다음 단계에서 **Shutdown virtual machine and perform a planned migration with no data loss?** 항목에 **Yes (4)**를 선택하고, **Capacity Reservation**에 **None (5)**을 선택한 후 **Migrate (6)**를 클릭하십시오.

    ![](./images/E3T4S15.png)

    ![](./images/E3T4S15a.png)

16. **UbuntuWAF** VM에서 **Action Pending (1)**을 선택하고 **Completion (2)**을 클릭한 후 **Migrate (3)**를 선택하십시오. 다음 단계에서 **Shutdown virtual machine and perform a planned migration with no data loss?** 항목에 **Yes (4)**를 선택하고, **Capacity Reservation**에 **None (5)**을 선택한 후 **Migrate (6)**를 클릭하십시오.

    ![](./images/E3T4S16.png)
    
    ![](./images/E3T4S16a.png)
       > **참고**: 약 15분이 소요됩니다.



17. **Notification (1)**에서 마이그레이션 상태가 완료되었는지 확인할 수 있습니다. **Refresh (2)**를 클릭하면 Execution Status가 **Completed (3)**로 표시됩니다.

    ![](./images/E3T4S17.png)

     > **축하합니다!** 작업을 완료하셨습니다! 이제 검증할 차례입니다. 다음 단계를 따라 주십시오:
     > - 해당 작업의 Inline Validate 버튼을 클릭하십시오. 성공 메시지를 받으시면 다음 작업으로 진행하실 수 있습니다.
     > - 성공하지 못한 경우, 오류 메시지를 주의 깊게 읽고 실습 가이드의 지침에 따라 단계를 다시 시도하십시오.
     > - 도움이 필요하시면 cloudlabs-support@spektrasystems.com으로 문의하십시오. 24시간 연중무휴로 지원해 드립니다.

     <validation step="db5d197a-151d-4987-8011-9568dc93e642" />

### 요약

이 실습에서는 VM 데이터 마이그레이션을 위한 Azure Storage Account를 생성하였습니다. Hyper-V 호스트(LabVM)를 Migration and Modernization 서비스에 등록하고, 마이그레이션을 위해 Azure Site Recovery를 사용하였습니다. Hyper-V 호스트에 Azure Site Recovery Provider를 배포하고 온프레미스 VM에서 Azure Migrate Server Migration 서비스로의 복제를 구성하였습니다. 복제된 VM의 고정 프라이빗 IP가 온프레미스 구성과 일치하도록 설정하였습니다. 마지막으로 UbuntuWAF, smarthotelweb1 및 smarthotelweb2 VM을 Azure로 성공적으로 마이그레이션하였습니다.

---

**[← 이전: 실습 2 - 비즈니스 가치 및 ROI 정당성 평가](Exercise2.md)** | **[다음: 실습 4 - GitHub Copilot을 활용한 애플리케이션 모더나이제이션 →](Exercise4.md)**
