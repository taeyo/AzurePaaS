## 3. 백엔드 서비스로 Windows 서비스를 사용하고 있었다면 대안이 필요해요

웹 응용프로그램은 가끔 백엔드 작업들을 수행하기 위해서(예를 들면, 데이터베이스 관련 작업이나 시스템 마무리 작업 등) Windows 서비스를 사용합니다. 물론, IaaS 방식으로 가상 컴퓨터(VM)에서 응용프로그램을 호스트하는 경우에는 Windows 서비스를 사용할 수 있긴 하지만, 그 방식은 아주 깔끔한 방식이라 할 수 없으며, PaaS 환경에서는 Windows 서비스를 아예 사용할 수 없습니다.

## 해결방안

첫번째 대안은 CQRS(Command Query Responsibility Segregation) 패턴과 함께 작업자 역할(Worker Role)을 사용하는 것입니다. CQRS 패턴에 대한 설명은 다음 링크를 참고하시기 바랍니다. 이는 조회 작업과 변경 작업을 분리하여 수행하는 패턴이며, 주로 클라우드 환경에 적합한 패턴이라 할 수 있습니다. 

> https://msdn.microsoft.com/en-us/library/azure/dn568103.aspx  
> http://blog.aliencube.org/ko/2013/08/18/cqrs-oversimplified-overview/ (한글, [유정협 MVP](https://twitter.com/justinchronicle) 작성)

다른 대안으로는 Web Jobs을 들 수 있습니다. 이는 웹 응용프로그램(Web App)에서 백그라운드 작업이 필요한 경우에 효과적인 방안이라 할 수 있습니다. 게다가, 다양한 실행 파일을 지원하기에 매우 유연합니다. 지원하는 파일 형식은 다음과 같습니다.
.cmd, .bat, .exe(windows cmd 사용), .ps1(powershell 사용), .sh(bash 사용), .php(php 사용), .py(python 사용), .js(node 사용), .jar(java 사용)
다만, 이는 Web App과 동일한 가상 컴퓨터(VM)에서 작업이 수행되므로 대규모 프로세스를 수행하는 것은 효과적이지 않을 수 있습니다. Web Job에 대한 자세한 설명은 다음 링크를 참고하시기 바랍니다.

> https://azure.microsoft.com/ko-kr/documentation/articles/web-sites-create-web-jobs/    
> https://azure.microsoft.com/ko-kr/documentation/articles/websites-webjobs-resources/  

만일, 처리해야 할 작업이 대규모이고 더 많은 컴퓨팅 리소스가 필요하다면 Azure 배치(Batch)의 사용을 고려해 보시기 바랍니다. 

> Azure Batch .NET 자습서  
> https://azure.microsoft.com/ko-kr/documentation/articles/batch-dotnet-get-started/ 
