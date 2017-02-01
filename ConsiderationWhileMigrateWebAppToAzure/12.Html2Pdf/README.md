## 12. HTML을 PDF로 변환하는 데에 제약이 있습니다

많은 웹 사이트에서 HTML을 PDF로 변환하여 사용자가 다운로드 할 수 있도록 하는 기능을 제공하고 있을텐데요. 
그러한 기존의 기능이 Azure Web App에서는 올바로 동작하지 않을 수 있습니다.

일반적으로 그러한 변환 기능은 대부분 상용(혹은 무료) 라이브러리를 구입 및 사용해서 구현을 하는데요.
대부분의 그러한 라이브러리들(Windows/.NET 용)이 내부적으로 IE API를 활용하며 결과적으로 USER32/GDI32 API를 
사용하게 되는데 이러한 API 호출은 Web App 환경에서 막혀있기 때문에 올바로 동작하지 않는 것입니다. 
물론, User32/GDI32를 내부적으로 활용하지 않는 상용 라이브러리들은 올바로 동작할 수도 있습니다.

## 해결방안

현재는 Web App의 Sandbox 구조로 인해서 이러한 제약이 있습니다만, 추후에는 개선이 될 가능성도 없지는 않습니다. 하지만, 현재 시점에서 반드시 HTML -> PDF 변환이 필요하다면 
그를 지원하는 제품을 사용하셔야 합니다. 다음은 글 작성시에 PDF 변환에 실패하는 제품과 성공하는 제품의 목록입니다.

PDF 변환에 실패한 제품
- Syncfusion
- Siberix
- NReco (wkhtmltopdf를 사용)
- Spire.PDF

PDF 변환에 성공한 제품
- SQL Reporting framework : 앱이 '기본' 가격계층 이상을 사용해야 함.
- EVOPDF: http://www.evopdf.com/azure-html-to-pdf-converter.aspx
- Telerik reporting: 앱이 '기본' 가격계층 이상을 사용해야 함.
- Rotativa / wkhtmltopdf: 앱이 '기본' 가격계층 이상을 사용해야 함.

