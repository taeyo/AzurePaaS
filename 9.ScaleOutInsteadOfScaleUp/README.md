##9. Scale-up이 아닌 Scale-out을 고려해야 해요

일반적으로 VM의 용량(크기)를 늘리는 방식(Scale-up)으로는 최적의 성능을 내기 어렵습니다. 이는 가동 중지 시간도 유발할 수 있으며 예상보다 만족할만한 성능을 즉각적으로 얻기도 쉽지 않습니다.

##해결방안

가능하다면 언제나 웹 응용프로그램의 인스턴스를 여러 개로 늘려서 분산처리하는 Scale-out 을 선택해야 합니다. IaaS에서는 이를 위해서는 2대 이상의 VM을 만들어서 가용성 집합을 구성하고 VM들의 앞 단에 부하분산 장치를 붙이는 형태의 설계가 필요합니다. 그리고 이러한 설계는 모든 클라우드 기반의 인프라에서 기본적인 권장 설계이기도 합니다. 더불어, PaaS에서는 이미 WebApp이 이를 위한 기반을 제공하므로 매우 쉽게(Azure Portal에서) 수평 확장 설정을 할 수 있습니다. 수동으로 확장할 수 있을 뿐만 아니라 CPU나 기타 메트릭의 수치 변화에 따라 자동으로 확장하는 Auto-Scale 기능도 사용할 수 있습니다. Azure 앱 서비스에 수평 확장을 적용하는 방법은 다음 문서를 참고하시기 바랍니다.

Azure 앱 서비스에서 웹 앱 크기 조정     
https://azure.microsoft.com/ko-kr/documentation/articles/web-sites-scale/

또한, 어느 정도의 규모로 Scale-out하는 것이 가장 효과적인지를 검증하기 위해서는 클라우드 기반의 부하 테스트도 고려하실 것을 권장합니다.  Azure 기반의 Visual Studio Load Test는 이러한 규모 대응 계획을 수립할 때 큰 도움이 되는 부하 테스트 기법입니다. 참고로, 2015년에 슈퍼서울콘서트가 이와 같은 테스트를 거쳐서 성공적으로 사전 규모 진단을 수행하고 예매 대란을 해결한 사례가 있습니다.

> 2015 슈퍼 서울 콘서트, MS 애저로 예매 대란 해결
>
>http://news.joins.com/article/19249462  
>http://www.datanet.co.kr/news/articleView.html?idxno=95277 

Visual Studio Load Test에 대한 상세한 설명은 다음 링크를 참고하시기 바랍니다.

https://msdn.microsoft.com/library/vs/alm/test/performance-testing/getting-started/getting-started-with-performance-testing 
