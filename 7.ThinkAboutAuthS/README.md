##7. 인증과 권한 허가에 대해 고려해야 합니다

온-프레미스 웹 응용프로그램을 클라우드로 이전하는 경우에 간과하고 넘어가기 쉬운 부분 중 하나는 Windows 통합 인증입니다. 여러분의 응용프로그램에서 Windows 통합 인증을 사용하고 있었다면 이 부분은 클라우드로 이전할 경우 상당히 고전하게 되는 부분입다. 그렇기에 적절한 계획이 필요합니다.

##해결방안

직접적인 해법은 Azure Active Directory를 Directory Sync와 함께 사용하는 것입니다. 하지만 이 경우 OAuth, SAML 등을 사용해야 하기 때문에 코드 변경이 뒤따를 수 밖에 없습니다. 사실 마이그레이션을 할 경우 가장 큰 난관이 바로 이 부분입니다. ASP.NET 웹 응용프로그램에서 Azure Active Directory를 통해 인증하는 방법은 다음 링크를 살펴보시기 바랍니다. 

https://azure.microsoft.com/ko-kr/documentation/articles/web-sites-authentication-authorization/

