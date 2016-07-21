# 한글 ActiveX Control API
 - http://www.hancom.com/menual.menualView.do?menuFlag=1&board_seqno=1

## Action object
사용자가 하나의 단위 기능으로 인식하는 글의 기능 각각을 액션이라고 부른다. 예를 들면 “파일열기”, “파일저장”, “표 삽입”, “글자 속성 수정” 등이다. 가장 쉽게 이해할 수 있는 액션의 정의는, '메뉴, 툴바, 단축키를 통해 실행할 수 있는 하나의 기능'으로 생각하면 된다. 기존 윈도우즈 어플리케이션에서 WM_COMMAND 핸들러에서 처리하는 개개의 단위 기능을 떠올리면 된다. 결국 사용자에게 어플리케이션은 이러한 단위 액션들의 집합으로 인식된다. HwpAction은 글 컨트롤을 이용하는 사용자가 글의 액션을 직접 다룰 수 있도록 하기 위한 오브젝트이다.

각 Prototype에서 명시한 파라메터의 타입의 종류는 다음과 같다.

|Type|설명|VARIANT ttype|member of VARIANT|
|---|---|---|---|
|BSTR|문자열|VT_BSTR|bstrVal|
|boolean/BOOL|BOOL|VT_BOOL|boolVal|
|byte|1 byte unsigned char|VT_UI1|bVal|
