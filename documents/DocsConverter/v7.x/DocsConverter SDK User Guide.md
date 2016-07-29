# 변환 SDK 사용 가이드
* 작성일: 2016-01-29

## 파일 구성
|경로|파일명|설명|
|:---|:---|:---|
|$INSTALLPATH/| |설치 루트 디렉터리|
| |convert.sh|변환 스크립트|
| |install-hnc.sh|패키지 설치 스크립트|
| |uninstall-hnc.sh|패키지 제거 스크립트|
|$INSTALLPATH/tfo| | TFO 변환필터 디렉터리|
| |hermes-processing-tfo-7.0.jar|TFO 변환필터 jar 파일|
| |lib/Java 라이브러리 파일 디렉터리|
| |jni/JNI 라이브러리 파일 디렉터리|
| |conf/|설정 파일 디렉터리|
|$INSTALLPATH/hncfilter| |HNC 변환필터 디렉터리|
| |hermes-processing-hnc-6.0.jar|HNC 변환필터 jar 파일|
| |libhncfilter/|Java 라이브러리 파일 디렉터리|
| |util/|pagecounter 유틸리티 디렉터리|
|$INSTALLPATH/packages/| |설치용 RPM 패키지 파일 디렉터리|
|$INSTALLPATH/phantomjs/| |PhantomJS Html2Pdf 변환 엔진|
|$INSTALLPATH/libs/| |HTML Viewer용 라이브러리 디렉터리|

## 설치
### 환경
아래 환경에서 테스트 되었습니다.
  - CentOS 6.7 Final (64bit)
  - Java 1.7 이상
  - ImageMagick 6.7.2-7

### 추가 구성요소 설치
* ImageMagick 패키지 설치
```
# yum -y install ImageMagick
```

3D효과를 표현하려면 X Window System 환경이어야 하며 Minimal OS 설치를 한 경우에는 Xvfb 패키지를 설치하여 그 기능을 대신 수행할 수 있습니다. 3D효과 지원이 안되는 경우 2D로 대체되기 때문에 필수 구성요소는 아닙니다.

* Xvfb 패키지 설치
```
# yum -y install xorg-x11-server-Xvfb
```

### 변환필터 설치
파일을 필요한 디렉터리에 위치시킨 후, 아래와 같이 스크립트를 실행합니다.
```
# install-hnc.sh
```

설치 스크립트를 실행하면, packages/ 아래의 패키지들이 시스템에 설치됩니다. 
이후 convert.sh 변환 스크립트를 통해서 변환기능을 사용할 수 있습니다.

### HTML 공통 라이브러리 복사
libs 폴더를 Web root 폴더에 복사합니다. (http://web-server/libs 경로)
자세한 설명은 [Html Viewer](#Html-Viewer)에 설명이 되어 있습니다.

## 사용법
다음과 같이 명령 프롬프트로 실행합니다.
```
$ convert.sh <input file> <output directory> <output type> [-D<key=value> -D<key=value> -D<key=value> ...]
```

입력 형식이 잘못된 경우 다음과 같이 도움말이 출력됩니다.
```
## Usage ##
convert.sh INPUT_FILE OUTPUT_DIR OUTPUT_TYPE [-Dkey=value –Dkey=value ...]
    Input: hwp, doc/docx, ppt/pptx, xls/xlsx
    Output: html, image, pdf, text

## Optional Arguments ##
    -DfirstPage           [Conversion first page]
    -DlastPage            [Conversion last page]
    -DmaxPage             [Conversion max page]
...
```

### 파라미터 설명
#### 필수 파라미터

명령 프롬프트에서 순서대로 입력합니다.

<필수 파라미터>
|이름|유형|설명|
|---|---|---|
|INPUT_FILE|string|입력 파일의 경로입니다.|
|OUTPUT_DIR|string|결과 파일을 출력할 디렉터리의 경로입니다.경로가 존재하지 않을 경우 새로 생성합니다.|
|OUTPUT_TYPE|string|입력 문서를 변환할 대상 포맷입니다.(html, pdf, image, text)|

#### 옵션 파라미터

명령 프롬프트에서 -D이름=값 형식으로 입력합니다. 입력하지 않을 경우 기본 값이 사용됩니다.

<공통 변환 옵션>
|이름|유형|설명|
|---|---|---|
|firstPage|integer|변환 시작 페이지를 지정합니다.변환 범위를 지정하여 변환할 경우 사용합니다. 지정하지 않을 경우 문서의 첫 페이지부터 변환합니다.|
|lastPage|integer|변환 종료 페이지를 지정합니다. 변환 범위를 지정하여 변환할 경우 사용합니다. 지정하지 않을 경우 문서의 마지막 페이지까지 변환합니다.|
|maxPage|integer|변환할 최대 페이지 수를 지정합니다. firstPage, lastPage 옵션과 함께 사용할 경우 maxPage 옵션이 우선적으로 적용됩니다. 지정하지 않을 경우 firstPage~lastPage 내 모든 페이지를 변환합니다.|
|imgType|string|이미지로 변환할 경우 포맷을 지정합니다.(jpg, png) 지정하지 않을 경우 jpg 포맷으로 변환합니다.|
|pageSplit|boolean|html 변환 시 페이지를 나눌지 여부를 결정합니다.<ul><li>true: 페이지 당 1개의 html 파일로 변환합니다.</li><li>false: 총 1개의 html 파일로 변환합니다. 지정하지 않을 경우 기본 값은 false입니다.</li></ul>|

<XLS/XLSX 변환 옵션>

|이름|유형|설명|
|---|---|---|
|xlsExportPageType|string|<ul><li>xls 파일을 변환하는 경우, 이미지 출력 방식을 선택합니다.</li><li>sheet: 1개의 시트 당 1개의 이미지로 변환합니다.</li><li>print: 쪽 나눔 당 1개의 이미지로 변환합니다.</li><li>tile: 잘게 자른 이미지로 변환합니다. 지정하지 않을 경우 기본 값은 sheet입니다.</li></ul>|
|maxCol|integer|xls 파일을 html 변환하는 경우, 최대 출력 열수를 지정합니다. 지정하지 않을 경우 전체 시트를 변환합니다.|
|maxRow|integer|xls 파일을 html 변환하는 경우, 최대 출력 행수를 지정합니다. 지정하지 않을 경우 전체 시트를 변환합니다.|


<HWP 변환 옵션>

|이름|유형|설명|
|---|---|---|
|requestXmlPath|string|hwp 파일을 제어하는 경우, 수행할 액션을 담은 XML 파일의 경로를 지정합니다.|
|pdfCompatible|boolean|hwp를 html로 변환시 phantomJS 와 호환성을 적용한다. phatomJS에서 SVG Fill Pattern을 완벽히 지원하지 못하므로 html 생성시 SVG Fill Pattern용 이미지를 사용하지 않고 미리 랜더링된 이미지를 사용하도록 한다.|


<워터마크 적용 옵션>

|이름|유형|설명|
|watermarkTemplate|string|hwp/doc/ppt/xls 문서를 html로 변환시 사용가능하며, SDK의 conf 폴더에 있는 template 파일의 이름을 지정합니다. template 파일은 “파일명.properties” 의 형태로 되어 있으며 수정 또는 추가를 할 수 있습니다. template 파일은 이미지 또는 텍스트의 속성에 대한 정보를 가지고 있습니다. - watermark1 : conf/watermark1.properties 파일을 사용하여 워터마크를 생성합니다. - watermark2 : conf/watermark2.properties 사용|






### 변환 예시
* HTML 변환
  - sample1.hwp 파일을 HTML로 변환합니다.

  ```
  $ convert.sh sample1.hwp output/ html
  ```

  변환 결과: output/document.html

  - sample1.hwp 파일을 HTML로 변환합니다. 

  ```
  $ convert.sh sample1.hwp output/ html –DpageSplit=true
  ```

  pageSplit 옵션에 따라, 페이지 당 1개의 파일로 변환됩니다.
  변환 결과: output/document_0001.htm, output/document_0002.htm, ... 

  - sample2.xlsx 파일을 HTML로 변환합니다.
  
  ```
  $ convert.sh sample2.xlsx output/ html –DmaxCol=100
  ```

  maxCol 옵션에 따라, 최대 100행까지 변환합니다.
  변환 결과: output/document.html

  - sample1.hwp 파일을 HTML로 변환하고 워터마크를 생성합니다.

  ```
  $ convert.sh sample1.hwp output/ html –DwatermarkTemplate=watermark1
  ```

  SDK의 conf 폴더에 있는 watermark1.properites를 템플릿 파일로 사용하여 변환된 html 파일에 워터마크를 적용합니다.
  변환 결과: output/document.html

* PDF 변환
  - sample3.docx 파일을 PDF로 변환합니다.
  ```
  $ convert.sh sample3.docx output/ pdf
  ```
  
  변환 결과: output/document.pdf

  - sample1.html 파일을 PDF로 변환합니다.
  
  ```
  $ convert.sh sample1.html output/ pdf
  ```
  
  HTML을 PDF 로 변환하는 것은 PhantomJS 엔진을 사용하는 기능으로 예시에 사용된 pdf 파라미터를 제외한 추가 변환 옵션은 필요하지 않습니다. 변환 결과: output/document.pdf

* 이미지 변환
  - sample4.ppt 파일을 이미지로 변환합니다.
  
  ```
  $ convert.sh sample4.ppt output/ image –DimgType=png
  ```
  imgType 옵션에 따라, 파일을 png 포맷으로 변환합니다.
  변환 결과: output/document_0001.png, output/document_0002.png, ... 

  - sample5.xlsx 파일을 이미지로 변환합니다.
  ```
  $ convert.sh sample5.xls output/ image –DxlsExportPageType=print
  ```
  
  imgType 옵션이 없으므로, 파일을 jpg 포맷으로 변환합니다. xlsExportPageType 옵션에 따라, 파일을 인쇄영역에 맞추어 이미지로 변환합니다.
  변환 결과: output/document_0001.jpg, output/document_0002.jpg, ... 

* 텍스트 변환
  - sample6.ppt 파일을 텍스트로 변환합니다.
  
  ```
  $ convert.sh sample6.ppt output/ text
  ```
  
  변환 결과: output/document.txt

* 전자결재용 필터 변환
  - sample6.hwp 파일의 "SignImg" 필드에 이미지를 삽입하고, "strReqName", "strReasonMemo" 필드의 텍스트 내용을 추출합니다. 다음과 같이 request xml 파일을 생성합니다.
  ```xml
<request>
    <actions>
        <action id="insertImage">
            <param id="fieldName">SignImg</param>
            <param id="imagePath">/path/to/image.jpg</param>
            <param id="insertType">0</param>
        </action>
        <action id="getTextValue">
            <param id="fieldName">strReqName</param>
        </action>
        <action id="getTextValue">
            <param id="fieldName">strReasonMemo</param>
        </action>
    </actions>
</request>
```xml

  그리고 다음과 같이 호출합니다.

  ```
  $ convert.sh sample6.hwp output/ hwp –DrequestXmlPath=/path/to/request.xml
  ```

  변환 결과:  output/document.hwp (이미지가 들어간 HWP 파일), 
              output/document.xml (텍스트 내용이 담긴 XML 파일)

  XML 파일은 아래와 같이 생성됩니다.
```xml
<responce>
    <return-vals>
        <return-val field="strReqName">이름</return-val>
        <return-val field="strReasonMemo">Insert MEMO Message</return-val>
    <return-vals>
</responce>
```

## Html Viewer
### 기본 파일/폴더 설명
<생성 파일/폴더>

|이름|설명|
|hview.html|<ul><li>컨테이너 html</li><li>사용자 브라우저에 URL 형식으로 전달</li><li>document.html 과 같은 경로에 생성되며 고정된 파일명이므로 hview.html을 항상 사용</li></ul>|
|document.html (document_####.html)|<ul><li>컨텐츠 html</li><li>hview.html 내부에서 iframe 영역에 표시</li><li>document.html은 싱글 변환 파일명이며 –DpageSplit 옵션으로 페이지 분할 변환한 경우에는 document_####.html 형식의 파일명으로 생성</li></ul>|
|document.json|<ul><li>hview.html에서 컨텐츠의 정보를 알아내는데 필요한 Json 파일</li></ul>|
|document.files|<ul><li>document.html (document_####.html)에서 컨텐츠 표시에 사용되는 리소스 디렉토리</li></ul>|



<라이브러리 폴더>

|이름|설명|
|---|---|
|libs|<ul><li>hview.html 의 UI/동작에 필요한 라이브러리 폴더</li><li>javascript, css, image 로 구성</li><li>hview.html 의 커스터마이징은 해당 폴더의 파일을 수정</li><li>현재 디폴트로 web root 기준으로 /libs 경로에 폴더 복사</li><li>설치는 SDK 배포 파일에 libs 폴더를 /libs 경로로 복사</li><li>설치 확인은 http://<domain>/libs/logo.png URL을 브라우저에 입력시 한컴 로고 이미지가 정상적으로 보이는지 확인</li></ul>|

<전체 파일 구조>

|이름|설명|
|---|---|
|http://<domain>/libs|고정된 라이브러리 폴더|
|http://<domain>/path-1/.../output/hview.html|변환된 HTML 파일과 같은 경로에 위치|


### 커스터마이징
<libs 폴더내 파일들>

|공통 라이브러리|필터별 라이브러리|필터명|
|---|---|---|
|<ul><li>hview.js</li><li>template.js</li><li>template.css</li></ul>|<ul><li>hwp.js</li><li>hwp.css</li></ul>|HWP|
| |<ul><li>write.js</li><li>write.css</li></ul>|DOC|
| |<ul><li>show.js</li><li>show.css</li></ul>|PPT|
| |<ul><li>calc.js</li><li>calc.css</li></ul>|XLS|

* hview.js
  hview.html 의 뷰어 동작에 필요한 코드입니다.

* template.js/css
  기본 템플릿 코드로 전반적인 UI 구현을 하고 있으며, 기본적인 커스터마이징에 사용됩니다.

* hwp.js/css
  template.js/css를 기본으로 HWP 뷰어에 추가 커스터마이징을 할 수 있습니다.

* write.js/css
  template.js/css를 기본으로 DOC 뷰어에 추가 커스터마이징을 할 수 있습니다.

* calc.js/css
  template.js/css를 기본으로 XLS 뷰어에 추가 커스터마이징을 할 수 있습니다.

* show.js/css
  template.js/css를 기본으로 PPT 뷰어에 추가 커스터마이징을 할 수 있습니다.
