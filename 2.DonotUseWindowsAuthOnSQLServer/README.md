##2. SQL 서버에서 Windows 인증을 사용하지 마세요

SQL 서버와의 연결에 Windows 인증을 사용하는 것은 수많은 LOB(Line of Business, 업무용) 응용프로그램에서 상당히 보편적입니다. 접근 및 설정이 용이하기 때문입니다. 간혹, 어떤 이들은 백엔드 SQL 데이터베이스로의 접근을 제한하기 위해서 액티브 디렉토리(AD) 보안 그룹을 활용하기도 하는데, 이를 사용하면 데이터베이스로의 접근과 다양한 보안 수준을 정의하기 위해서 추가적으로 코딩을 해야 할 필요가 없기 때문입니다. 하지만, 여러분이 PaaS 기반으로 웹 응용프로그램을 이전하는 경우에는 이러한 방식이 항상 제대로 동작하지는 않을 수도 있습니다. 예를 들자면, SQL Azure 데이터베이스는 Windows 인증을 지원하지 않습니다.

##해결방안

가능하다면 SQL 인증을 사용하십시오. 만일, 여러분의 데이터베이스를 Azure SQL 데이터베이스로 이전한다면 사용 가능한 인증 방법은 SQL 인증 밖에 없기 때문입니다. Azure SQL 데이터베이스와 SQL Server (on-premises)의 차이점에 대해서는 다음의 공식 문서를 참고해 보시기 바랍니다.

https://azure.microsoft.com/ko-kr/documentation/articles/data-management-azure-sql-database-and-sql-server-iaas/ 

간혹, SQL Server는 온-프레미스에 존재하는 머신을 그대로 사용하지만 웹 응용프로그램은 Azure 상에서 운용하고 싶은 경우도 있는데 이 경우에는 설정이나 보안과 관련하여 다양한 제약이 발생할 수 있습니다. 이러한 경우의 대안으로는 WCF 서비스와 함께 “서비스 버스 릴레이”를 사용하는 방안을 고려할 수도 있습니다. 자세한 내용은 다음의 링크들을 참고하시기 바랍니다.

>Azure 서비스 버스 릴레이 서비스를 사용하는 방법  
>https://azure.microsoft.com/ko-kr/documentation/articles/service-bus-dotnet-how-to-use-relay/ 
>
>Connecting on-premises SQL Server using Azure Service Bus Relay    
>https://blogs.msdn.microsoft.com/wriju/2015/07/09/connecting-on-premises-sql-server-using-azure-service-bus-relay/ 
