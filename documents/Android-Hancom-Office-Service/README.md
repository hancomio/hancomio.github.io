# 한컴오피스 서비스
* 한컴오피스가 설치된 안드로이드 폰에서 한컴 오피스를 서비스로 사용해 문서 변환을 수행할 수 있습니다.

### 한컴오피스 문서 변환 서비스 for Android
* 업데이트 일자 : 2015.01.14

#### 기본 기능
* Extract from Office document
  * APC / T supported document
  * Office page to image files
  * Extract page extra information.(Hyperlink, embeded files, Slide note, Sheet information)
* Export to Office document
  * PPTX and PDF format supported.
  * Image files to Office document.

#### Extract API
* HOfficeService API
  * initialize()
  * newDocumentAsync()
  * setTotalPageNum()
  * insertImageAsync()
  * insertPageAfterAsync()
  * saveDocumentAsync()
  * unInitialize()
  * cancel()
* Listener(HOfficeListener)
  * onInitialize()
  * onNewDocument()
  * onInsertImage()
  * onInsertPage()
  * onSaveDocument()

#### Export to Office document
* HOfficeService API
  * initialize()
  * newDocumentAsync()
  * setTotalPageNum()
  * insertImageAsync()
  * insertPageAfterAsync()
  * saveDocumentAsync()
  * unInitialize()
  * cancel()
* Listener(HOfficeListener)
  * onInitialize()
  * onNewDocument()
  * onInsertImage()
  * onInsertPage()
  * onSaveDocument()

#### Extract sequence

#### Export sequence

#### 전달 구성 및 환경-1
* B2B Service-release-3rd party.pdf
  * 동작 개요 및 API 문서
* hanoffice-service-1.23_javadoc.zip
  * API Java doc 문서(영문)
* hanoffice-service-1.23.jar
  * service JAR library trial version
* HISTORY.TXT
  * release document
* HncB2BService.zip
  * Sample project
   
* 기본 환경
  * Device : Hancom Office 설치된 삼성 Device
  * 추천: Galaxy Tab 10.1
    * 전달된 hanoffice-service-1.22-trial.jar를 개발 Project\libs\ 에 위치한 후 개발 진행
