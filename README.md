# Azure PaaS 개발 시 주의해야 할 사항들

여러분의 웹 응용프로그램을 Azure의 PaaS 환경으로 이전할 계획을 가지고 있다거나, 혹은 새롭게 클라우드 기반의 Web App(웹 앱)을 제작할 계획을 가지고 있다면 다음의 다양한 조건들을 사전에 검토해 보시기 바랍니다. 

기본적으로 웹 응용프로그램이 분산 환경을 지원하도록 설계되어 있다면 50% 이상은 이전할 준비가 되어 있는 것입니다만, 다른 계층(Layer)의 애플리케이션과의 연동을 위해서는 소스의 변경이나 아키텍처의 재설계가 요구될 수도 있습니다. 

이 문서에서는 다양한 고려사항들을 하나씩 차례대로 살펴보도록 하겠습니다.
시작 전에 한 가지 언급하자면, 여러분의 웹 응용프로그램이 웹 팜(WebFarm) 환경을 지원하도록 설계되었다면 이하의 고려 사항 중 많은 부분은 이미 충족되어 있을 것이라는 점입니다. 그렇기에, 무엇보다 먼저 웹팜(여러 대의 웹 서버에 응용프로그램을 분산 배치하여 운영하는 설계)을 지원하도록 응용프로그램을 설계하시기 바랍니다.

1. [In-Proc 세션은 사용하지 마세요](/1.DonotUseInProcSession/) 
2. [SQL 서버에서 Windows 인증을 사용하지 마세요](/2.DonotUseWindowsAuthOnSQLServer/)
3. [백엔드 서비스로 Windows 서비스를 사용하고 있었다면 대안이 필요해요](/3.YouNeedAlternativeForWindowService/)
4. [파일 업로드를 서버의 로컬 경로에 하면 안돼요](/4.FileUpload/)
5. [설치형 3rd-party 컴포넌트는 설치할 수가 없어요](/5.CannotInstall3rdControls/)
6. [응용프로그램에 맞게 지원되는 적절한 플랫폼을 선택해야 합니다](/6.CheckSupportedApplicationPlatform/)
7. [인증과 권한 허가에 대해 고려해야 합니다](/7.ThinkAboutAuthS/)
8. [SSL을 제공해야 합니다](/8.UseSSL/)
9. [Scale-up이 아닌 Scale-out을 고려해야 해요](/9.ScaleOutInsteadOfScaleUp/) 
10. [서버의 UTC 시간을 고려해야 해요](/10.UTCTimezone/)
11. [WebApp은 인바운드 IP와 아웃바운드 IP가 다릅니다](/11.InboundIPNotSameOutboundIP/)


  
> 참고 문서     
__Top 10 things while migrating existing Web Applications to Azure__    
https://blogs.msdn.microsoft.com/wriju/2015/07/07/top-10-things-while-migrating-existing-web-applications-to-azure/ 