---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-14"

---

{:new_window: target="blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 사용자 액세스 관리
{: #managing-user-access}

구성원 대시보드에서 {{site.data.keyword.iot_full}} 조직에 대한 액세스를 제어하고 관리할 수 있습니다. 액세스를 추가, 초대<!--, registering--> 또는 가져오기하여 사용자를 추가할 수 있습니다. 또한 역할을 지정하여 사용자에 대해 다양한 액세스 레벨을 부여할 수도 있습니다.
{:shortdesc}

## 사용자 추가
{: #adding-new-users}

대시보드의 **구성원** 탭에서 추가 기능을 사용하여 개별 구성원을 추가할 수 있습니다. 또한 가져오기 기능을 사용하여 여러 구성원을 동시에 추가할 수 있습니다. 

기본적으로, 구성원 계정은 만료되지 않습니다. 사용자를 {{site.data.keyword.iot_short_notm}} 조직에 추가할 때는 해당 계정에 대한 만료 날짜를 선택사항으로 설정할 수 있습니다.

### {{site.data.keyword.iot_short_notm}} 조직에 구성원 추가

조직에 구성원을 추가하려면 구성원의 IBM ID가 필요합니다. 구성원 추가를 위해 구성원의 확인을 받을 필요는 없습니다.

{{site.data.keyword.iot_short_notm}} 조직에 단일 구성원을 추가하려면 다음을 수행하십시오.
1. {{site.data.keyword.iot_short_notm}} 대시보드에서 **구성원**으로 이동하십시오.
2. **구성원 추가**를 클릭하고 **추가** 탭을 선택하십시오.
3. 구성원의 IBM ID를 입력하십시오.
4. 구성원의 역할을 선택하십시오.
5. 선택사항: 구성원의 만료 날짜를 설정하십시오.
6. **추가**를 클릭하십시오.

여러 구성원을 동시에 추가하려면 IBM ID, 역할 및 각 구성원의 만료 날짜가 포함된 `.csv` 파일을 업로드해야 합니다. 관련 정보는 [CSV 파일 생성](#constructing-your-csv)을 참조하십시오.
1. {{site.data.keyword.iot_short_notm}} 대시보드에서 **구성원**으로 이동하십시오.
2. **구성원 추가**를 클릭하고 **가져오기** 탭을 선택하십시오.
3. 파일을 찾아보거나 `.csv` 파일을 **CSV 업로드** 창으로 끌어오십시오.
4. CSV 파일에 지정된 역할이 인식되지 않는 경우 사용할 기본 역할을 선택하십시오.
5. CSV 파일의 열 번호를 해당하는 IBM ID, 역할 및 (선택사항) 만기 날짜 항목에 맵핑하십시오.
6. `.csv` 파일에 사용된 구분 기호와 일치하도록 적절한 쉼표 또는 세미콜론 열 구분 기호를 선택하십시오.
7. IBM ID를 가져오고 구성원을 작성하려면 **가져오기**를 클릭하십시오.

<!--
### Inviting members to your {{site.data.keyword.iot_short_notm}} organization

When you invite a user to become a member of your {{site.data.keyword.iot_short_notm}} organization, the user receives an email that contains an invitation link. Invitation links expire 48 hours after they are sent. If an invitation link is not used within 48 hours, the user must be invited again to receive a new invitation link.

**Important:** The invite feature requires a configured mail service. For more information, see the Email section of the [External service integrations](reference/extensions/index.html#email) topic.

To invite a member to your {{site.data.keyword.iot_short_notm}} organization:
1. In the {{site.data.keyword.iot_short_notm}} dashboard, go to **Members**.
2. Select the **Invitations** tab.
2. Click **Invite Members** and select the **Invite** tab.
3. Enter the email address of the member.
4. Select a role for this member.
5. Optional: Set an expiry date for the member.
6. Click **Invite Member**.

To invite multiple members simultaneously, you must upload a `.csv` file that contains the email address, role and the optional expiry date of each member. For information, see [Constructing your CSV file](#constructing-your-csv).
1. In the {{site.data.keyword.iot_short_notm}} dashboard, go to **Members**.
2. Select the **Invitations** tab.
2. Click **Invite Members** and select the **Import** tab.
3. Browse your files or drag the `.csv` file into the **Upload CSV** window.
4. Select a default role to use if a role specified in the CSV file is not recognized.
5. Map the column numbers in your CSV file to the corresponding email address, role, and (optional) expiry date entries.
6. Select the appropriate comma or semicolon column separator to match the separator used in your `.csv` file.
7. Click **Import** to send out the invitations. -->

<!-- ### Registering a member with your {{site.data.keyword.iot_short_notm}} organization

If your organization is using {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.ssoshort}}, you can add individual members to your organization by registering them, which does not require an IBMid.

To register a member with your {{site.data.keyword.iot_short_notm}} organization:
1. In the {{site.data.keyword.iot_short_notm}} dashboard, go to **Members**.
2. Select the **Invitations** tab.
2. Click **Invite Members** and select **Invite**.
3. Enter the email address of the member.
4. Select a role for this member.
5. Enter the subject, realm name, and issuer.
   **Important:** Ensure that the `Subject`, `Realm Name`, and `Issuer` fields comply with the OpenID Connect recommendations and standards. For more information, see the [OpenID Connect ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://openid.net/connect/){: new_window} website.
6. Optional: Set an expiry date for the member.
7. Click **Register Member**.

To register multiple members simultaneously, you must upload a CSV (`.csv`) file that contains the email address, role, subject, realm name, issuer, and the optional expiry date of each member.
1. In the {{site.data.keyword.iot_short_notm}} dashboard, go to **Access**.
2. Click **Add Member** and select **Import**.
3. Click **Bulk Register**.
4. Select a default role and ensure that the column numbers on your CSV file match the column numbers in the CSV settings.
5. Ensure the column separator in your CSV file matches the column separator in the CSV settings.
6. Click **Browse your files** or drag the CSV file into the **Upload CSV** window. -->

### CSV 파일 생성
{: #constructing-your-csv}

조직에 구성원을 가져오기 위해 CSV 파일을 생성할 때는 사용 중인 메소드에 대한 필수 정보를 반드시 포함해야 합니다. 쉼표 또는 세미콜론을 사용하여 열을 분리하십시오.  
**중요:** CSV 파일을 업로드하는 경우 **CSV 설정** 아래에서 올바른 열 구분 기호를 선택했는지 확인하십시오.

샘플 CSV 파일(쉼표 구분 기호 사용):  
```
user1@sample.com,PD_DEVELOPER_USER,2018-03-13
user2@sample.com,PD_OPERATOR_USER,2018-03-13
user3@sample.com,PD_ADMIN_USER,2018-03-13
```
다음 열 항목이 사용되는 위치:  
- 1열: 사용자의 이메일 주소.  
구성원을 추가하는 경우 올바른 IBM ID에 해당하는 이메일 주소를 사용해야 합니다. 구성원을 초대하는 경우에는 모든 올바른 이메일 주소를 사용할 수 있습니다.
- 2열: 사용자에게 지정될 역할.  
다음 역할 중 하나를 입력하십시오.
 - PD_ADMIN_USER
 - PD_OPERATOR_USER
 - PD_DEVELOPER_USER
 - PD_ANALYST_USER
 - PD_READER_USER  
사용자 역할에 대한 자세한 정보는 [사용자, 애플리케이션 및 게이트웨이 역할](roles_index.html#user_roles)을 참조하십시오.
- 3열: 사용자 만료 날짜에 해당하는 ISO 6801 날짜(예: `2018-03-13`).

## 사용자 편집
{: #editing-users}

사용자를 편집하여 해당 역할을 변경하고 액세스 만기 날짜를 추가, 제거 또는 변경하거나 조직에 대한 액세스 권한을 추가 또는 제거할 수 있습니다.

1. {{site.data.keyword.iot_short_notm}} 대시보드의 왼쪽에 있는 탐색줄에서 **구성원**을 클릭하십시오.
2. 편집하려는 사용자 옆의 **편집** 아이콘을 클릭하십시오.
3. 원하는 대로 사용자를 변경하십시오.
4. **저장**을 클릭하십시오.

## 사용자 액세스 차단
{: #blocking-users}

조직 멤버십은 그대로 유지하면서 {{site.data.keyword.iot_short_notm}} 조직에 액세스하지 못하도록 사용자를 차단할 수 있습니다.

1. {{site.data.keyword.iot_short_notm}} 대시보드의 왼쪽에 있는 탐색줄에서 **구성원**을 클릭하십시오.
2. {{site.data.keyword.iot_short_notm}} 조직에 액세스하지 못하도록 차단할 사용자 옆의 토글 단추를 클릭하십시오.

## 사용자 액세스 제한
{: #limiting-users}

리소스 레벨 액세스 제어는 조직 내에서 디바이스에 대한 액세스를 제한할 수 있습니다. 리소스 그룹을 사용하여 각 사용자 또는 API 키가 관리할 수 있는 디바이스를 지정할 수 있습니다. 리소스 레벨 액세스 제어 구성 방법에 대한 정보는 [리소스 레벨 액세스 제어 구성](reference/rlac.html#configure_RLAC)을 참조하십시오.

## 사용자 제거
{: #removing-users}

사용자를 {{site.data.keyword.iot_short_notm}} 조직에서 완전히 제거할 수 있습니다. 사용자 제거는 취소될 수 없으며, 액세스 권한을 복원하려면 사용자를 [플랫폼에 추가](#adding-new-users)해야 합니다.

1. {{site.data.keyword.iot_short_notm}} 대시보드의 왼쪽에 있는 탐색줄에서 **구성원**을 클릭하십시오.
2. 제거할 사용자 옆의 **삭제** 아이콘을 클릭하십시오.
3. 확인 대화 상자에서 **삭제**를 클릭하십시오.
