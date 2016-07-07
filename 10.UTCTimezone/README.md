##10. 서버의 UTC 시간을 고려해야 해요

Azure App Service의 모든 PaaS 응용프로그램은 서로 다른 지역(Region)의 서로 다른 데이터센터에서 운영된다 하더라도 동일한 시간대를 갖습니다. 즉, 지역에 무관하게 모두 세계 표준시(UTC)를 따른다는 것이며, 일본에 있는 데이터센터라고 해서 일본 시간대를 따르거나, 미국 동부에 있는 데이터센터라고 해서 미국 동부 시간대를 따르지 않는다는 것입니다. 그렇기에, 기존의 응용프로그램이 이러한 시간을 중요하게 다루고 있다면 접속한 최종 사용자의 지역에 따라 적절히 변환하여 보여주어야 합니다. 
특히, SQL Azure를 사용할 경우에는 심도 있게 고려해야 합니다. SQL Azure도 마찬가지로 UTC가 기본 시간대이고 getdate() 시에 UTC 시간을 가져오게 됩니다. 문제는 대부분의 기존 응용프로그램들이 Local 시간을 데이터베이스에 기록해왔다는 것입니다. 그렇기에, Azure로 전환한 이후에는 기존 날짜 데이터와 UTC 날짜 데이터가 섞이게 될 수 있으면 식별이 어려워질 수 있습니다. 그렇기에, 날짜 데이터를 일관되게 다루기 위한 전략이 필요합니다.

##해결방안

UI 측면에서는 웹 응용프로그램에서 다음과 같은 변환 코드(C#의 예)를 사용하여 시스템의 UTC 시간을 로컬 시간에 맞게 출력해 주도록 합니다. 

```{cs}
DateTime convertedDate = DateTime.SpecifyKind(
    DateTime.Parse(dateStr),
    DateTimeKind.Utc);
DateTime dt = convertedDate.ToLocalTime();
```

SQL Azure를 사용할 경우에는 기존의 Local 시간 데이터와 이제부터의 UTC 시간 데이터의 Time Zone을 맞추기 위한 정책이 필요합니다. 다음 중의 한 가지 방법을 선택하여 사용할 수 있습니다만, 이는 공식 권장사항은 아니며 커뮤니티에서 주로 거론되는 방식이기에 참고로 살펴보시기 바랍니다.

1.	기존에 로컬 시간으로 저장해 두었다면 그를 모두 UTC로 변경한다
2.	TimeZone 관련 컬럼을 추가하여 기존 날짜 데이터(Local)와 신규 날짜 데이터(UTC)를 구분할 수 있도록 한다. 즉, 기존 날짜 데이터들에 대해서는 타임존 컬럼에 Local을 기록하고, 신규 시간 데이터들은 UTC를 기록한 뒤, 그에 따라 응용프로그램 서버에서 시간 데이터를 변환하여 사용하는 것이다.
3.	UTC 기간을 로컬 시간에 맞게 변환하여 저장하여 사용한다.

어떠한 방식을 사용하던 여러분의 상황에 맞는 방안을 결정하여 사용하시기 바랍니다. 다음은 참고할만한 문서입니다.

Manage TimeZone for Applications on Windows Azure      
https://blogs.msdn.microsoft.com/cie/2013/07/29/manage-timezone-for-applications-on-windows-azure/

Managing Timezone in SQL Azure  
http://wely-lau.net/2011/07/10/managing-timezone-in-sql-azure-2/ 
