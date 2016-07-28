# 한글 ActiveX Control API
 - http://www.hancom.com/menual.menualView.do?menuFlag=1&board_seqno=1

## Action object
사용자가 하나의 단위 기능으로 인식하는 한/글의 기능 각각을 액션이라고 부른다. 예를 들면 “파일열기”, “파일저장”, “표 삽입”, “글자 속성 수정” 등이다. 가장 쉽게 이해할 수 있는 액션의 정의는, '메뉴, 툴바, 단축키를 통해 실행할 수 있는 하나의 기능'으로 생각하면 된다. 기존 윈도우즈 어플리케이션에서 WM_COMMAND 핸들러에서 처리하는 개개의 단위 기능을 떠올리면 된다. 결국 사용자에게 어플리케이션은 이러한 단위 액션들의 집합으로 인식된다. HwpAction은 글 컨트롤을 이용하는 사용자가 글의 액션을 직접 다룰 수 있도록 하기 위한 오브젝트이다.

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

### 속성(Property)
#### ActID[읽기전용]
액션 ID를 나타낸다.
* 구문(Syntax)
  C++
  ```cpp
  CString Getactid()
  ```

  javascript
  ```javascript
  (string) ActID
  ```
* 설명(Remarks)
액션 ID의 종류 및 설명은 별도 문서 참조.
 
* 예제(Example)
* 참고 항목(See Also)
#### SetID[읽기전용]
액션이 사용하는 ParameterSetID를 나타낸다
* 구문(Syntanx)
  C++
  ```cpp
  CString Getsetid()
  ```
  
  javascript
  ```javascript
  (string)SetID
  ```
* 설명(Remarks)
parameter set을 사용하지 않는 액션은 빈 문자열을 리턴한다.
* 예제(Example)
* 참고항목(See Also)

### 메소드(Method)
#### CreateSet
액션과 대응하는 parameter set을 생성한다.
* 구문(Syntanx)
  C++
  ```cpp
  LPDISPATCH CreateSet()
  ```
  
  javascript
  ```javascript
  ParameterSet CreateSet()
  ```
* 매개변수(Parameters)
* 반환값(Return)
  해당 Action과 연관되는 ParmeterSet Object.
* 설명(Remarks)
  parameter set을 사용하지 않는 액션의 경우 NULL이 리턴된다.
  이 method는 다음과 같이 수행한 것과 동일하다.
  ```Set param = HwpCtrl.CreateSet(action.SetID)```
* 예제(Example)
* 참고항목(See Also)

#### GetDefault
  현재 상태에 따라 액션 실행에 필요한 인수를 구한다.
* 구문(Syntanx)
  C++
  ```cpp
  void GetDefault(LPDISPATCH set)
  ```
  
  javascript
  ```javascript
  GetDefault(ParameterSet set)
  ```
* 매개변수(Parameters)
  set :인수를 저장할 ParameterSet
* 반환값(Return)
  해당 Action과 연관되는 ParmeterSet Object.
* 설명(Remarks)
  예를 들어 글자모양의 액션의 경우, 현재 셀렉션 상태에 따라 param의 아이템들이 채워진다.
  서브셋을 만들 경우에는 서브셋을 만든 후에 GetDefault를 사용한다.
* 예제(Example)
* 참고항목(See Also)

#### PopupDialog
  액션의 대화상자를 띄운다.
* 구문(Syntanx)
  C++
  ```cpp
  long PopupDialog(LPDISPATCH set)
  ```
  
  javascript
  ```javascript
  number PopupDialog(ParameterSet set)
  ```
* 매개변수(Parameters)
  set : 여기에 지정한 아이템의 값에 따라 대화상자의 각 컨트롤의 초기 값이 결정되고, 대화상자가 닫힌 후에는 사용자가 지정한 값들이 담겨 돌아온다.
* 반환값(Return)
  해당 Action과 연관되는 ParmeterSet Object.
* 설명(Remarks)
  액션이 정의하기에 따라 다르지만, 일반적으로 다음과 같은 modal dialog result를 리턴한다.

  |ID|값|설명|
  |---|---|---|
  |hwpOK|IDOK|다이얼로그 박스의 확인버튼을 눌렀을 경우 리턴 되는 값|
  |hwpCancel|IDCANCEL|다이얼로그 박스의 취소버튼을 눌렀을 경우 리턴 되는 값|
  |hwpError|-1|실행시 에러가 발생 하였을 경우 리턴 되는 값|
  
* 예제(Example)
* 참고항목(See Also)

#### Execute
  지정한 인수로 액션을 실행한다.
* 구문(Syntanx)
  C++
  ```cpp
  long Execute(LPDISPATCH set)
  ```
  
  javascript
  ```javascript
  number Execute(ParameterSet set)
  ```
* 매개변수(Parameters)
  set : 액션의 실행을 제어할 인수. parameter set의 종류와 아이템의 의미는 액션이 정의한 바에 따라 다르다.
* 반환값(Return)
  액션이 성공하면 1, 실패하면 0을 반환한다.
* 설명(Remarks)
* 예제(Example)
* 참고항목(See Also)

#### Run
  액션을 실행한다.
* 구문(Syntanx)
  C++
  ```cpp
  void Run()
  ```
  
  javascript
  ```javascript
  Run()
  ```
* 매개변수(Parameters)
* 반환값(Return)
* 설명(Remarks)
  ```CreateSet```, ```GetDefault```, ```PopupDialog```, ```Execute```를 차례로 부른 것과 같다.
  또, 다음 두 가지도 동일하다.
   ```HwpCtrl.Run "action"```, ```HwpCtrl.CreateAction("action").Run```
* 예제(Example)
* 참고항목(See Also)

