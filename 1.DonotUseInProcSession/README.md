##1. In-Proc 세션은 사용하지 마세요

세션(Session)은 웹 개발에서는 매우 일반적으로 사용되는 개념입니다. 일반적으로 세션은 로컬 웹 서버에서 처리하도록 프로그래밍 되는 편이고 서버의 외부에서는 접근할 수 없습니다. 하지만, 분산 환경에서는 서로 다른 여러 대의 웹 서버에 응용프로그램을 배포하고 앞 단에 부하 분산기를 배치하여, 사용자의 요청을 분산 처리하게 구성하곤 합니다. 이러한 설계에서는 특정 웹 서버가 다운되거나 장애가 발생한다 하더라도 부하 분산기가 알아서 가용한 다른 웹 서버를 파악하여 그를 통해 요청을 처리해주는 가용성을 확보할 수 있게 됩니다. 하지만, 이러한 분산 설계 하에서는 세션과 관련하여 문제가 발생할 가능성이 있는데, 예를 들면 사용자가 처음에는 A 머신에 접속하여 로그인을 수행하여 세션을 맺었지만, 그 다음 요청은 B 머신으로 접속하게 될 수 있으며 이 경우 세션이 없는 것처럼 인식되어 다시 로그인을 수행해야 할 수 있습니다. 그렇기에, 분산 환경에서는 세션을 웹 서버에서 직접 관리(즉, In-Proc)하는 것이 아니라 별도로 관리하는 방안(예, 세션 서버)이 필요합니다.

또한, 기본적으로 클라우드 환경에서는 성능 확장을 위해서 스케일업(수직 확장, VM 혹은 App의 물리적 능력을 높이는 방안)보다는 스케일아웃(수평 확장, VM 혹은 App을 여러 개 추가하는 구성)을 고려해야 하기 때문에 세션 관리에 대한 부분은 반드시 대비해야 하는 사항이기도 합니다. 스케일아웃에 대한 부분은 이 문서의 9번 항목에서 다루고 있습니다.

참고 : [Scale-up이 아닌 Scale-out을 고려해야 해요](https://github.com/taeyo/AzurePaaS/blob/master/9.ScaleOutInsteadOfScaleUp)


##해결방안

일반적으로 세션의 상태(Session State)를 저장하기 위해서 파일 서버, 데이터베이스 서버, 캐시 서버 등이 활용되곤 합니다. 즉, 상태를 관리하는 별도의 서버를 두고 모든 세션은 그 서버를 통해서 관리하도록 한다는 것입니다. 이렇게 구성하면 웹 서버가 여러 대로 구성되는 웹팜(WebFarm) 환경에서도 매끄럽게 세션을 관리할 수 있습니다.
Azure 에서는 상태 서버로 활용할 수 있는, 관리되는 캐시 서비스인 Redis Cache를 제공하고 있습니다.  물론 원한다면, 별도의 파일 서버나 RDBMS를 상태 서버로 활용할 수 있지만 최근의 추세는 기업 수준의 응용프로그램에서는 Redis Cache를 사용하는 것이 권장됩니다. 
ASP.NET 웹 응용프로그램의 경우는 기본적으로 .NET 프레임워크에서 Session State Provider가 제공되기에 다음과 같은 방안을 사용하여 매우 쉽게 Redis Cache 상태 서버를 구성할 수 있습니다.

> Azure Redis Cache에 대한 ASP.NET 세션 상태 제공자   
> [https://azure.microsoft.com/ko-kr/documentation/articles/cache-asp.net-session-state-provider/](https://azure.microsoft.com/ko-kr/documentation/articles/cache-asp.net-session-state-provider/) 

> Azure 앱 서비스에서 Azure Redis 캐시를 사용하는 세션 상태  
> [https://azure.microsoft.com/ko-kr/documentation/articles/web-sites-dotnet-session-state-caching/](https://azure.microsoft.com/ko-kr/documentation/articles/web-sites-dotnet-session-state-caching/) 

다만, Java 웹 응용프로그램의 경우에는 별도의 Session State Provider가 존재하지 않기에 오픈소스에서 지원되는 기술들을 검토해 보아야 합니다. Tomcat 서버를 사용하는 경우에는 Memcached Session Manager라는 모듈을 사용하여 고가용성을 확보할 수 있습니다. Memcached Session Manager는 memcached 기술을 사용한 고가용성 클러스터입니다. 이를 사용하여 상태를 관리하는 방안은 다음 링크들을 참고하시기 바랍니다.

> https://code.google.com/archive/p/memcached-session-manager/  
> https://github.com/magro/memcached-session-manager 

반드시 이러한 기술을 사용해야만 하는 것은 아닙니다. 이미 여러분의 온-프레미스에서 상태 서버를 관리 및 운용하고 있다면 그를 Azure에 가상 컴퓨터(VM)으로 구성하여 여전히 활용할 수 있습니다. 
