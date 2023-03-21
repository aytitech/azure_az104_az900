# 4. Kısım Sonu Alıştırmaları

1: Yeni bir **Azure AD Kullanıcısı** oluşturun. Ama henüz bununla oturum açmayın.
<details>
  <summary>Cevabı görmek için genişletin</summary>

- Portalde arama kısmına "Azure Active Directory" yazın ve "Azure AD" yönetim ekranına geçin.
- Sol menüden "Users" kısmına tıklayın. Açılan ekranda "New User>Create New User" yolunu takip edin. 
- Ekrandaki kullanıcı adı vb. bilgileri doldurarak kullanıcı oluşturma adımlarını tamamlayın.
</details>

***
2: Azure CLI araclığıyla iki adet **Resource Group** yaratın. Grupların ismi **GroupA** ve **GroupB** olsun.
<details>
  <summary>Cevabı görmek için genişletin</summary>

```shell
$ az group create --name "GroupA" --location westeurope
$ az group create --name "GroupB" --location westeurope
```

</details>

***

3: İlk adımda oluşturduğunuz kullanıcıya **GroupA** isimli **Resource Group** üstünde **Contributor**, **GroupB** isimli **Resource Group**  üstünde **Reader** rollerini atayın.
<details>
  <summary>Cevabı görmek için genişletin</summary>

- Portalde arama kısmına "Resource Group" yazın ve "Resource Group" yönetim ekranına geçin.

- Burada "GroupA" isimli grubu bulun ve tıklayın.

- Açılan ekranda soldaki menüde "Access control (IAM)" ekranına geçin ve "Add>Add Role Assignment" yolunu takip edin. Açılan listeden "Contributor" rolünü seçin ve next ile devam ederek bir sonraki ekranda "+Select Member" butonuyla birinci alıştırmada oluşturduğunuz kullanıcıyı seçin ve işlemleri tamamlayın. 

- Tekrar "Resource Group" yönetim ekranına geçin ve burada "GroupB" isimli grubu bulun ve tıklayın.

- Açılan ekranda soldaki menüde "Access control (IAM)" ekranına geçin ve "Add>Add Role Assignment" yolunu takip edin. Açılan listeden "Reader" rolünü seçin ve next ile devam ederek bir sonraki ekranda "+Select Member" butonuyla birinci alıştırmada oluşturduğunuz kullanıcıyı seçin ve işlemleri tamamlayın.

</details>

***

4: Hem **Azure Portal** hem de **Azure Cli** üstünde ilk adımda oluşturduğunuz kullanıcıyla oturum açın. Bundan sonraki adımları bu kullanıcı ile uygulayacaksınız.
<details>
  <summary>Cevabı görmek için genişletin</summary>

- Tarayıcınızın incognito mode özelliğini kullanarak yeni bir sayfa açın ve portal.azure.com adresinden Azure Portal’a birinci alıştırmada oluşturduğunuz kullanıcı ile oturum açın.
- Azure Cli’da ```az logout``` komutuyla oturumu sonlandırın ve ardından tekrar ```azure login``` komutunu kullanın ve birinci alıştırmada oluşturduğunuz kullanıcı ile oturum açın.

</details>

***

5: Yeni bir **Resource Group** oluşturmayı deneyin. Oluşturabildiniz mi?
<details>
  <summary>Cevabı görmek için genişletin</summary>

Kullanıcısınız Subscription seviyesinde yetkisi olmadığından dolayı resource group oluşturamadınız. 
```shell
$ az group create --name "GroupC" --location westeurope
Code: AuthorizationFailed
```

</details>

***

6: **Azure CLI** aracılığıyla **GroupA** isimli **Resource Group** içerisinde bir **Sanal Makine** yaratın.
<details>
  <summary>Cevabı görmek için genişletin</summary>

Aşağıdaki komutu girdiğiniz zaman size "Admin Password:" yazısı olarak şifre oluşturmanız için bir adım çıkacak. Buraya 2 defa belirlediğiniz en az 12 haneli ve içerisinde büyük harf, küçük harf, rakam ve işaret olan bir şifre giriniz.

```shell
$ az vm create --resource-group "GroupA" --name "ilksanalmakine" --image Win2019Datacenter --location westeurope
```

</details>

***

7: Aynı **Sanal Makine** yaratma işlemini bu sefer **GroupB** isimli **Resource Group** üstünde deneyin.
<details>
  <summary>Cevabı görmek için genişletin</summary>

"code":"AuthorizationFailed" hatası alarak oluşturamamanız gerekir. Çünkü bu kullanıcının GroupB üstünde sadece Reader rolü mevcut.

```shell
$ az vm create --resource-group "GroupB" --name "ilksanalmakine" --image Win2019Datacenter --location westeurope
```

</details>

***

8: Tekrar kendi kullanıcınızla oturum açın ve **Subscription** bazında Delete aksiyonlu bir **Resource Lock** oluşturun.
<details>
  <summary>Cevabı görmek için genişletin</summary>

- Yeni bir tarayıcı ekranı açarak kendi kullanıcınız ile oturum açın. 

- Portalde arama kısmına "Subscriptions" yazın ve "Subscription" yönetim ekranına geçin.

- Soldaki menüden "Resource Locks" ekranına gelin. +Add butonuna basın ve açılan ekranda "Resource Locks" için isim olarak "dellock" girin ve "Lock Type" olarak "Delete" aksiyonunu seçin.

</details>

***

9: **GroupA** isimli **Resource Group**u silmeyi çalışın.
<details>
  <summary>Cevabı görmek için genişletin</summary>

- Portalde arama kısmına "Resource Group" yazın ve "Resource Group" yönetim ekranına geçin.

- Burada "GroupA" isimli grubu bulun ve tıklayın.

- Orta üst menüdeki "Delete Resource Group" butonuna basın ve açılan ekranda "TYPE THE RESOURCE GROUP NAME:" kısmına "GroupA" yazın ve "Delete" butonuna basın. 

- "The resource group GroupA is locked and can't be deleted. Click here to manage locks for this resource group" uyarısı alacak ve silemeyeceksiniz.

</details>

***
10: Oluşturduğunuz **Resource Lock**’u **Azure Cli** aracılığıyla kaldırın.
<details>
  <summary>Cevabı görmek için genişletin</summary>

```shell
$ az lock delete --name dellock
```

</details>

***

11: **GroupA** isimli **Resource Group**’a *departman=hr* olarak **Tag** atayın.
<details>
  <summary>Cevabı görmek için genişletin</summary>

- Portalde arama kısmına "Resource Group" yazın ve "Resource Group" yönetim ekranına geçin.

- Burada "GroupA" isimli grubu bulun ve tıklayın.

- Soldaki menüden "Tags" ekranına gelin. Name kısmına "departman", Value kısmına "ik" değerlerini girip kaydedin.

</details>

***

12: Bu **Tag**’in bu **Resource Group** altındaki kaynaklara inherit etmediğini görün. Bunun olmasını sağlayacak **Policy**’i devreye alın ve bu **Tag**lerin oluştuğunu görün.
<details>
  <summary>Cevabı görmek için genişletin</summary>

- Portalde arama kısmına "Resource Group" yazın ve "Resource Group" yönetim ekranına geçin.

- Burada "GroupA" isimli grubu bulun ve tıklayın.

- "ilksanalmakine" isimli sanal makineye tıklayın. Soldaki menüden "Tags" ekranına gelin. Burası boş olmalı. Resource Group'a tag atandığında altındaki kaynaklara da otomatik olarak aynı tag atanmaz.

- Portalde arama kısmına "Policy" yazın ve "Azure Policy" yönetim ekranına geçin.

- Sol menüdeki "Assigments" kısmına tıklayın ve açılan ekranda "Assign Policy" butonuyla devam edin. 

- "Scope" kısmında "GroupA" seçin. "Policy Definition" kısmında ... tıklayarak "Add a tag to resources" policy'sini seçin ve next ile devam edin.

- Üst menüden "Parameters" kısmına gelin ve departman-ik değerlerini ilgili ekranlara girin.

- Üst menüden "Remediation" kısmına gelin ve "Create a remediation task" checkbox'ını işaretleyi ve ardından "Review+Create" butonuyla devam ederek policy'i oluşturun.

- Bir süre bekleyin "24 saat sürebilir max.". Sonrasında ilksanalmakine isimli sanal makinenizi bulun ve "Tags" kısmından tag’in eklendiğini teyit edin. 

</details>

***

13: Yeni bir **Management Group** oluşturun.
<details>
  <summary>Cevabı görmek için genişletin</summary>

- Portalde arama kısmına "Management groups" yazın ve "Management groups" yönetim ekranına geçin.

- Karşınıza çıkan ekrandaki "+Create" butonuna basın. Açılan ekranda "Management group ID" kısmına "001", "Management group display name" kısmına da "Alistirma_Management_Group" yazın Submit butonuna tıklayarak oluşturma işlemini tamamlayın.

</details>

***

14: Mevcut **Subscription**’ızı oluşturduğunuz **Management Group** altına taşıyın.
<details>
  <summary>Cevabı görmek için genişletin</summary>

- Portalde arama kısmına "Management groups" yazın ve "Management groups" yönetim ekranına geçin.

- Ana ekranda "Tenant Root Group" altında size ait "Subscription" göreceksiniz. Bu satırın sağ kısmında ... butonu var, buna tıklayın ve "Move" butonuna basın.

- Açılan ekranda "Alistirma_Management_Group" seçili olacak şekilde "Save" butonuna basın ve taşıma işlemini tamamlayın.

</details>

***

15: Sadece *A* ve *B* serisi **Sanal Makineler** oluşturulabilsin, başka tipte bir **Sanal Makine** oluşturulamasın şeklinde bir **Policy** tanımı yapın ve bunu oluşturduğunuz **Management Group**’a atayın.
<details>
  <summary>Cevabı görmek için genişletin</summary>

- Portalde arama kısmına "Policy" yazın ve "Azure Policy" yönetim ekranına geçin.

- Sol menüdeki "Assigments" kısmına tıklayın ve açılan ekranda "Assign Policy" butonuyla devam edin.

- "Scope" kısmına tıklayın ve "Alistirma_Management_Group" isimli "Management Group" seçin. "Policy Definition" kısmında ... tıklayarak "Allowed size SKUs" policy'sini seçin ve next ile devam edin.

- Üst menüden "Parameters" kısmına gelin "Allowed size SKUs" kısmında tüm A ve B serilerini seçin. "İlk 30 seçenek, Standard_D1'e kadar olan tüm seçenekler işaretlenecek". "Review+Create" butonuyla devam ederek policy'i oluşturun.

</details>

***

16: *D* serisi bir **Sanal Makine** oluşturmayı deneyin ve oluşturamadığınızı görün. Sonrasında da şu ana kadar yapmış olduğunuz iki "Policy Assigment" işlemini de geri alın. 
<details>
  <summary>Cevabı görmek için genişletin</summary>

- Aşağıdaki komutu "Azure CLI" girdiğiniz zaman size "Admin Password:" yazısı olarak şifre oluşturmanız için bir adım çıkacak. Buraya 2 defa belirlediğiniz en az 12 haneli ve içerisinde büyük harf, küçük harf, rakam ve işaret olan bir şifre giriniz. Ardından "The template deployment failed because of policy violation..." hatası alacaksınız. 

```shell
$ az vm create --resource-group "GroupA" --name "ikincisanalmakine" --image Win2019Datacenter --location westeurope --size Standard_DS1_v2

$ {"error":{"code":"InvalidTemplateDeployment","message":"The template deployment failed because of policy violation...
```

- Portalde arama kısmına "Policy" yazın ve "Azure Policy" yönetim ekranına geçin.

- Sol menüdeki "Assigments" kısmına tıklayın. Açılan ekranda önce "- Portalde arama kısmına "Policy" yazın ve "Azure Policy" yönetim ekranına geçin.

- Sol menüdeki "Assigments" kısmına tıklayın ve açılan ekranda "Add a tag to resources" policy'sinin sağ tarafında bulunan ... tıklayın ve "Delete Assigment" seçeneği ile devam ederek silin. Aynı işlemi "Allowed virtual machine size SKUs" policy'si için de yapın ve silin. 


</details>

***

17: **Subscription**’ızı tekrar **Root Management Group**’a taşıyın.
<details>
  <summary>Cevabı görmek için genişletin</summary>

- Portalde arama kısmına "Management groups" yazın ve "Management groups" yönetim ekranına geçin.

- Ana ekranda "Alistirma_Management_Group" altında size ait "Subscription" göreceksiniz. Bu satırın sağ kısmında ... butonu var, buna tıklayın ve "Move" butonuna basın.

- Açılan ekranda "Tenant Root Group" seçili olacak şekilde "Save" butonuna basın ve taşıma işlemini tamamlayın.

</details>

***

18: **Azure Cli** araclığıyla bir **Service Principal** oluşturun.
<details>
  <summary>Cevabı görmek için genişletin</summary>

Aşağıdaki komutu "Azure CLI" girdiğiniz zaman yeni bir "Service Principal" oluşturacaksınız. Bu komutun çıktısını bir yere not edin. 

```shell
$ az ad sp create-for-rbac --name ilkServicePrincipal 
```

</details>

***

19: Oluşturduğunuz **Service Principal**’in **GroupA** isimli **Resource Group** üstünde **Reader** rölüne sahip olmasını sağlayın.
<details>
  <summary>Cevabı görmek için genişletin</summary>

- Aşağıdaki komutu girin ve bu komutun çıktısındaki "id" değerinizi bir kere not edin. Bu sizin "Subscription Id" değeriniz.

```shell
$ az account show
{
  "environmentName": "AzureCloud",
  "homeTenantId": "5fccn384-ad28-4535-1234-cdf3b6cb9f83",
  "id": "a22ss2s9-3324-23s2-a8ef-92ce597fcb2f",
  "isDefault": true,
  "managedByTenants": [],
  "name": "Ayti.Tech Egitim Subscription",
  "state": "Enabled",
  "tenantId": "5fccn384-ad28-4535-1234-cdf3b6cb9f83",
  "user": {
    "cloudShellID": true,
    "name": "live.com#ategitim@hotmail.com",
    "type": "user"
  }
}
```

- Aşağıdaki komutta "appId" yerine oluşturduğunuz "Service Principal" appID değerini, "mySubscriptionID" yerine de "Subscription Id" değerinizi girin. Örn: ```az role assignment create --assignee 36fb113d-88d2-23f3-82f4-123874fae2b1 --role Reader --scope /subscriptions/a22ss2s9-3324-23s2-a8ef-92ce597fcb2f/resourceGroups/GroupA```
  
```shell
$ az role assignment create --assignee appID --role Reader --scope /subscriptions/mySubscriptionID/resourceGroups/GroupA
```

- Portalde arama kısmına "Resource Group" yazın ve "Resource Group" yönetim ekranına geçin.

- Burada "GroupA" isimli grubu bulun ve tıklayın.

- Açılan ekranda soldaki menüde "Access control (IAM)" ekranına geçin ve "Role assignments" kısmına gelin. Açılan ekranda "ilkServicePrincipal" isimli Service Principal'a "Reader" rolünün atandığını teyit edin. 

</details>

***

20: Oluşturduğunuz **Service Principal**’i kullanarak **Azure** ile sağladığı **Rest Api** üstünden haberleşin ve **GroupA** isimli **Resource Group** altındaki kaynakları listeleyin. “Postman ya da Curl kullanabilirsiniz”.
<details>
  <summary>Cevabı görmek için genişletin</summary>

- Azure Cloud Shell oturumu açın. "Eğer makinenizde Curl yüklü ise buna gerek yok, kendi komut satırınızdan da halledebilirsiniz". 

- Service Principal oluşturduğumuz adımda komutun çıktısını bir yere kaydedin demiştim. Şimdi buradaki bilgiler bize lazım olacak. Aşağıdaki komutta [APP_ID], [PASSWORD] ve [TENANT_ID] kısımlarını o çıktıdaki değerlerle değiştirerek bu komutu çalıştırın. Bu bizim client_credential flow'unu kullanarak Authorization Token'ımızı almamızı sağlayacak. Örnek Komut: ```curl -X POST -d 'grant_type=client_credentials&client_id=36fb113d-1234-1234-1234-2318741232b1&client_secret=123456Zko5IKYdMw~b6eFBJKeJIsP-vLcvHPaIa&resource=https%3A%2F%2Fmanagement.azure.com%2F' https://login.microsoftonline.com/123494cb4-1234-4148-1234-12345b9f83/oauth2/token```

```shell
curl -X POST -d 'grant_type=client_credentials&client_id=[APP_ID]&client_secret=[PASSWORD]&resource=https%3A%2F%2Fmanagement.azure.com%2F' https://login.microsoftonline.com/[TENANT_ID]/oauth2/token
```

- Bu komutun çıktısı bize aşağıdaki gibi bir json değer sağlayacak. Burada access_token'dan sonra gelen token değerini kopyalayın ve bir yere kaydedin. "eyJ0eX... ile başlayıp ...RNw ile biten kısım."
  
```shell
{"token_type":"Bearer","expires_in":"3599","ext_expires_in":"3599","expires_on":"1679335415","not_before":"1679331515","resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ii1LSTNROW5OUjdiUm9meG1lWm9YcWJIWkdldyIsImtpZCI6Ii1LSTNROW5OUjdiUm9meG1lWm9YcWJIWkdldyJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzVmYzk0Y2I0LWFmOTgtNDE0OC1iODhiLWNkZjNiNmNiOWY4My8iLCJpYXQiOjE2NzkzMzE1MTUsIm5iZiI6MTY3OTMzMTUxNSwiZXhwIjoxNjc5MzM1NDE1LCJhaW8iOiJFMlpnWUVnM1ZkbWY2aHNwdi91UE1iZmY4L01DQUE9PSIsImFwcGlkIjoiMzZmYjExM2QtMjJiNi00M2IyLThkZDQtMjMxODc0ZmFlMmIxIiwiYXBwaWRhY3IiOiIxIiwiaWRwIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvNWZjOTRjYjQtYWY5OC00MTQ4LWI4OGItY2RmM2I2Y2I5ZjgzLyIsImlkdHlwIjoiYXBwIiwib2lkIjoiY2JjMWY3YzMtMzU1OC00ZTlhLWFmODUtZmEwOWVhMzcxNmIxIiwicmgiOiIwLkFZSUF0RXpKWDVpdlNFRzRpODN6dHN1ZmcwWklmM2tBdXRkUHVrUGF3ZmoyTUJPVkFBQS4iLCJzdWIiOiJjYmMxZjdjMy0zNTU4LTRlOWEtYWY4NS1mYTA5ZWEzNzE2YjEiLCJ0aWQiOiI1ZmM5NGNiNC1hZjk4LTQxNDgtYjg4Yi1jZGYzYjZjYjlmODMiLCJ1dGkiOiJjOU5TZ3plekkwV3RZNTRwdUlWZUFBIiwidmVyIjoiMS4wIiwieG1zX3RjZHQiOjE2NjI3Mjk1NjB9.BsYmD4TdI76X2NZBqfMSbBTTJD_5aRb9XSMIUhi7so4zgA1WyW8n8onLar7pkcAIRCQS-yBlnGOHiKhiWH6IuIRPRCN5YacJvrb6lbXCVmO2dkoUCGGGVq3M6GrTkk5AUbSBLbtROi4hJKh_FWrbYpoSeYXSn9XWgw1VyF9Gbh_s2B-QhR9SsYKrS_onJdiaeafuOH13bZCupmWtmxxOMHeX8RdfGFXA9cHo8DjRkC9wyHlKXPZIKC2t22M8auR-5EVcGlF8IvIea2e0ZKrcEUAcK9TvY7sQiqz-9L7ymWhaygKyMOrB_T7-e6xoIes1UadrbqpPZSw1RNt06NpRNw"}
```

- Artık sorgumuzu göndermeye hazırız. Aşağıdaki komuttaki [Token] değerini silelim bir önceki adımda kopyaladığımız değeri yapıştıralım. [Subscription ID] değerini de Subscription ID'mizile değiştirelim. Bu komutun sonucu bizler "GroupA" altındaki tüm kaynakları json olarak geri döndürecek. Örnek Komut: ```curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ii1LSTNROW5OUjdiUm9meG1lWm9YcWJIWkdldyIsImtpZCI6Ii1LSTNROW5OUjdiUm9meG1lWm9YcWJIWkdldyJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzVmYzk0Y2I0LWFmOTgtNDE0OC1iODhiLWNkZjNiNmNiOWY4My8iLCJpYXQiOjE2NzkzMzE1MTUsIm5iZiI6MTY3OTMzMTUxNSwiZXhwIjoxNjc5MzM1NDE1LCJhaW8iOiJFMlpnWUVnM1ZkbWY2aHNwdi91UE1iZmY4L01DQUE9PSIsImFwcGlkIjoiMzZmYjExM2QtMjJiNi00M2IyLThkZDQtMjMxODc0ZmFlMmIxIiwiYXBwaWRhY3IiOiIxIiwiaWRwIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvNWZjOTRjYjQtYWY5OC00MTQ4LWI4OGItY2RmM2I2Y2I5ZjgzLyIsImlkdHlwIjoiYXBwIiwib2lkIjoiY2JjMWY3YzMtMzU1OC00ZTlhLWFmODUtZmEwOWVhMzcxNmIxIiwicmgiOiIwLkFZSUF0RXpKWDVpdlNFRzRpODN6dHN1ZmcwWklmM2tBdXRkUHVrUGF3ZmoyTUJPVkFBQS4iLCJzdWIiOiJjYmMxZjdjMy0zNTU4LTRlOWEtYWY4NS1mYTA5ZWEzNzE2YjEiLCJ0aWQiOiI1ZmM5NGNiNC1hZjk4LTQxNDgtYjg4Yi1jZGYzYjZjYjlmODMiLCJ1dGkiOiJjOU5TZ3plekkwV3RZNTRwdUlWZUFBIiwidmVyIjoiMS4wIiwieG1zX3RjZHQiOjE2NjI3Mjk1NjB9.BsYmD4TdI76X2NZBqfMSbBTTJD_5aRb9XSMIUhi7so4zgA1WyW8n8onLar7pkcAIRCQS-yBlnGOHiKhiWH6IuIRPRCN5YacJvrb6lbXCVmO2dkoUCGGGVq3M6GrTkk5AUbSBLbtROi4hJKh_FWrbYpoSeYXSn9XWgw1VyF9Gbh_s2B-QhR9SsYKrS_onJdiaeafuOH13bZCupmWtmxxOMHeX8RdfGFXA9cHo8DjRkC9wyHlKXPZIKC2t22M8auR-5EVcGlF8IvIea2e0ZKrcEUAcK9TvY7sQiqz-9L7ymWhaygKyMOrB_T7-e6xoIes1UadrbqpPZSw1RNt06NpRNw" -H "Content-Type: application/json" https://management.azure.com/subscriptions/a12347529-1234-46c0-a8ef-222ce597fcb2f/resourceGroups/GroupA/resources?pi-version=2021-04-01```

```shell
$ curl -X GET -H "Authorization: Bearer [Token]" -H "Content-Type: application/json" https://management.azure.com/subscriptions/[Subscription ID]/resourceGroups/GroupA/resources?api-version=2021-04-01
```

</details>

***

21: **GroupA** isimli **Resource Group** altında oluşturduğunuz **Sanal Makinede**, **System Assigned Managed Identity** hizmetini devreye alın. Bu **Managed Identity**’e **GroupB** isimli **Resource Group** üstünde **Reader** rölünü atayın.  
<details>
  <summary>Cevabı görmek için genişletin</summary>


- Portalde arama kısmına "Resource Group" yazın ve "Resource Group" yönetim ekranına geçin.

- Burada "GroupA" isimli grubu bulun ve tıklayın.

- "ilksanalmakine" isimli sanal makineye tıklayın. Soldaki menüden "Identity" ekranına gelin. Karşınıza çıkan ekranda "Status" kısmını Off seçeneğinden On seçeneğine döndürün ve "Save" butonuna basın. Bu sayede bu makineye bir "Managed Identiy" atanacak. 

- Portalde arama kısmına "Resource Group" yazın ve "Resource Group" yönetim ekranına geçin.

- Burada "GroupB" isimli grubu bulun ve tıklayın.

- Açılan ekranda soldaki menüde "Access control (IAM)" ekranına geçin ve "Add>Add Role Assignment" yolunu takip edin. Açılan listeden "Reader" rolünü seçin ve next ile devam ederek bir sonraki ekranda ilk olarak "Assign access to" kısmında "Managed Identity" seçin. Ardından "+Select Member" butonuyla "ilksanalmakine" isimli managed identity'i seçin ve işlemleri tamamlayın. 

</details>

***

22: 200 TL'lık bir **Budget** oluşturun ve bu **Budget**’a ulaşıldığı zaman size mail göndermesini sağlayacak işlemleri tamamlayın.
<details>
  <summary>Cevabı görmek için genişletin</summary>

- Portalde arama kısmına "Subscriptions" yazın ve "Subscription" yönetim ekranına geçin.

- Soldaki menüden "Budgets" ekranına gelin. +Add butonuna basın ve yeni bir budget oluşturma adımlarına başlayın. "Name" kısmına bir isim girin, "Reset Period" kısmı "Monthly" olarak seçilsin. "Creation date" ve "Expiration date" kısımlarını olduğu gibi bırakın. "Amount" kısmını TL ise 200, Euro ya da Dolar ise 10 değerini girin ve  

- Bir sonraki adımda "Alert condition" kısmında tip olarak "Actual" ve "% of budget" kısmına da 90 değerini girin. "Alert recipients (email)" kısmına da email adresinizi girip "Create" butonuyla devam edin. 

</details>

***

23: **Az PowerShell Module** kullanarak **GroupA** ve **GroupB** isimli **Resource Group**ları silin.
<details>
  <summary>Cevabı görmek için genişletin</summary>

```shell
> Remove-AzResourceGroup -Name "GroupA"                           

Confirm
Are you sure you want to remove resource group 'GroupA'
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
```

```shell
> Remove-AzResourceGroup -Name "GroupB"                           

Confirm
Are you sure you want to remove resource group 'GroupA'
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
```

</details>

***