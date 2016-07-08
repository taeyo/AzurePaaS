##11. WebApp은 인바운드 IP와 아웃바운드 IP가 다릅니다

Azure WebApp에서는 인바운드 IP와 아웃바운드 IP가 다릅니다. 동적으로 생성되는 인바운드 IP 주소를 통해서 아웃바운드가 일어나지 않는다는 사실을 인지하는 것이 중요합니다. 그렇기에, 여러분은 웹 서비스에서 외부 3rd-Party의 서비스를 호출한다 거나 해야 한다면 여러분의 WebApp이 가지고 있는 아웃바운드 IP 주소를 파악해서 이를 해당 3rd-Party 업체에 제공해야 할 필요가 있습니다. 대부분의 3rd-Party 기업은 자사의 서비스를 보호하기 위한 목적으로 방화벽 등의 보안 방안을 가지고 있을 것이기 때문입니다. 그렇기에, Azure WebApp에서 외부 API를 호출해야 할 일이 있는 경우에는 외부 API 제공자(3rd-Party) 쪽에서 우리의 아웃바운드 IP를 알려줘서 필요할 경우 자사의 방화벽에 등록하게끔 해야 합니다.

##해결방안

WebApp의 아웃바운드 IP 주소는 다음과 같이 속성 메뉴에서 확인이 가능합니다.

![참고 이미지](/images/webappOutboundIP.png)

다만, 이러한 아웃바운드 IP는 App Service Plan이 변경될 경우, 그에 따라 변경될 가능성이 높기에 주의해야 합니다. 또한, 그 외에도 알아두면 좋을만한 내용이 다음 문서에서 언급되고 있으니 참고하시기 바랍니다.

Azure Web Apps – Outgoing IP Question/Answer    
https://peterintheazuresky.wordpress.com/2016/02/26/azure-web-apps-outgoing-ip-questionanswer/
