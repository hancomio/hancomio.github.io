# 한글 ActiveX Control API
 - http://www.hancom.com/menual.menualView.do?menuFlag=1&board_seqno=1

## Action object
사용자가 하나의 단위 기능으로 인식하는 글의 기능 각각을 액션이라고 부른다. 예를 들면 “파일열기”, “파일저장”, “표 삽입”, “글자 속성 수정” 등이다. 가장 쉽게 이해할 수 있는 액션의 정의는, '메뉴, 툴바, 단축키를 통해 실행할 수 있는 하나의 기능'으로 생각하면 된다. 기존 윈도우즈 어플리케이션에서 WM_COMMAND 핸들러에서 처리하는 개개의 단위 기능을 떠올리면 된다. 결국 사용자에게 어플리케이션은 이러한 단위 액션들의 집합으로 인식된다. HwpAction은 글 컨트롤을 이용하는 사용자가 글의 액션을 직접 다룰 수 있도록 하기 위한 오브젝트이다.

각 Prototype에서 명시한 파라메터의 타입의 종류는 다음과 같다.

|Type|설명|VARIANT type|member of VARIANT|
|---|---|---|---|
|BSTR|문자열|VT_BSTR|bstrVal|
|boolean/BOOL|BOOL|VT_BOOL|boolVal|
|byte|1 byte unsigned char|VT_UI1|bVal|
|unsigned short|2 byte unsigned integer|VT_UI2|uiVal|
|unsigned long|4 byte unsigned integer|VT_UI4|ulVal|
|char|1 byte signed char|VT_I1|cVal|
|short|2 byte signed integer|VT_I2|iVal|
|long|4 byte signed integer|VT_I4|lVal|
|int|signed integer|VT_INT|intVal|
|unsigned int|unsigned integer|VT_UINT|uintVal|

* 글 액션을 이용하면서 주의해야할 점은 하나의 액션을 Create한 후 Execute하기 전까지 또 다른 액션을 Create하면 안된다. 예를 들어,
```
act1.CreateAction();
act2.CreateAction();
...
act1.Execute();
act2.Execute();
```
위와 같이 사용할 경우 잘못된 동작을 수행하게 되는 경우가 발생할 수 있다.

* Example - javascript
```javascript
var act;
var set;
act = HwpControl.HwpCtrl.CreateAction("PageSetup");// 액션 생성
set = act.CreateSet();// parameter set 생성
act.GetDefault(set);// parameter set 초기화
if (act.PopupDialog(set) == 1)// 대화상자 띄우기
act.Execute(set);// 액션 실행

```
