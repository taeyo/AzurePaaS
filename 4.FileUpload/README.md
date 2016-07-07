##4. 파일 업로드를 서버의 로컬 경로에 하면 안되요

대부분의 웹 응용프로그램은 파일 업로드 기능을 제공합니다. 즉, 이미지나 문서 파일을 서버로 업로드할 수 있는 기능을 제공하곤 합니다. 다만, 많은 웹 응용프로그램들이 업로드된 파일을 로컬 폴더(예, D:\Files 등)에 저장하는 방식을 취하는데, 이 방식은 클라우드의 PaaS와 같은 분산 환경에서는 문제가 발생할 수 있습니다. 즉, 웹 응용프로그램이 여러 대의 웹 서버에서 구동되는 경우에는 사용자가 업로드한 파일이 다른 서버에 존재하게 되는 문제가 생길 수 있기 때문입니다(이는 1번 세션 문제와 유사한 상황이라 할 수 있습니다). 그렇기에, 모든 파일들을 중앙 지점에서 관리할 수 있는 방안이 필요하며, 그에 따라 코드를 수정해야 할 수 있습니다.

##해결방안

가장 간편한 방안은 중앙 파일 저장소로 Azure Blob 저장소를 사용하는 것입니다. 이는 생각보다 사용하기에 매우 용이합니다. 저장소를 활용하는 예는 다음 문서들을 참고하시기 바랍니다. 다양한 프로그래밍 언어를 지원할 뿐만 아니라 사용하기도 용이합니다.

> https://azure.microsoft.com/ko-kr/documentation/articles/storage-dotnet-how-to-use-blobs/  
> https://azure.microsoft.com/ko-kr/documentation/articles/storage-java-how-to-use-blob-storage/ 


추가적으로, 참고할 만한 예제로써 웹 응용프로그램에서 Azure Blob으로 파일을 저장하고 불러오는 예제(HTML Form 버전과 Javascript Ajax 버전)를 만들어 보았습니다. 관련 샘플이 필요하신 분은 다음 링크의 예제를 확인해 보시기 바랍니다. 

__Azure 파일 업로드__   
https://github.com/jiyongseong/AzurePaaSHol/tree/master/AzureFileUploadWeb

다음은 예제의 간단한 설명입니다.

- 기본 예제는 ASP.NET MVC 기본 파일 업로드 방식으로 작성됨
    - ASP.NET MVC에서 그리드는 Grid.MVC를 활용
- 추가 예제는 jQuery.Form을 활용한 HTML/Javascript 파일 업로드 방식으로 작성 
    - 그리드는 Knockout을 활용하여 MVVM 으로 구현(Json 바인딩)
- 서버 측은 Java나 Php 등으로 구현해도 무방함(예에서는 서버로 ASP.NET을 활용함)
- 웹 페이지 혹은 스크립트를 통해서 업로드 되는 파일은 스트림 그대로 Azure Storage로 전송되도록 구현
- 예제 소스는 이해하기 쉽도록 동기(Sync) 메서드를 사용하여 구현하였음

구현 시에 고려한 사항은 다음과 같습니다.

- 모든 파일은 Azure Blob Storage의 특정 컨테이너에 저장한다.
- Azure Blob Storage의 특정 컨테이너는 읽기 전용으로 설정해야 한다.
- 사용자가 웹 서버로 업로드하는 파일은 스트림 그대로 Azure Storage 쪽에 저장해야 한다.
- 사용자가 파일 내역을 조회하고자 하는 경우에는 웹 서버는 파일 목록과 해당 파일로의 직접적인 하이퍼링크를 제공한다 
- 웹 서버가 파일 다운로드를 중계하는 방식은 권장하지 않는다(메모리 낭비 및 CDN 적용 불가)
- 사용자는 개별 파일에 대해서는 Azure Blob에 직접 접근하여 다운로드를 수행한다.
