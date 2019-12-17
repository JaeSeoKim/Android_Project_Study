# **Overview of App Widgets**

위젯이란 사용자의 홈 화면에서 바로 접근이 가능한, 앱의 '한눈에 보기'라고 할 수 있다. 사용자의 스타일에 따라 홈 화면에 맞춤설정을 할 수 있다. 홈 화면의 패널간 위젯을 자유로이 이동할 수 있으며 크기조절을 통해 정보량을 조절 가능한 위젯도 있다.



## Types of Widget

위젯의 종류는 다음과 같다.

- ### Information Widgets

  정보 위젯은 보통 사용자에게 중요한 정보를 표시한다. 시간에 따를 정보 변화를 추적한다. 날씨 위젯, 시계 위젯, 스포츠 경기 점수 위젯이 대표적인 예시이다. 일반적으로 이러한 위젯을 터치하면 연결된 앱이 실행되고 이에 맞는 상세정보가 나온다.

  

- ### Collection Widgets

  콜렉션 위젯은 갤러리의 사진, 뉴스 기사, 이메일, 메시지 등 동일한 유형의 여러 요소를 표시하기에 최적화 되어있다. 콜렉션 위젯은 일반적으로 콜렉션 둘러보기, 세부정보 뷰로 열기 등 두 가지 사용 예시에 중점을 둔다.

  

- ### Control Widgets

  컨트롤 위젯의 기본 목적은 빠른 실용성이다. 앱을 열 필요 없이 홈 화면에서 바로 트리거할 수 있는 자주 사용되는 함수를 표시하는 것이다. 컨트롤 위젯의 예시로는는 음악 재생, 일시중지, 곡이동 등이 가능한 음악 앱이다.

  

- ### Hybrid Widgets

  모든 위젯들은 위의 세 가지 종류 중 하나의 성향이 있지만 실제로는 대부분의 위젯들이 다양한 유형의 요소를 결합한 하이브리드 위젯이다. 예를들어 음악 앱의 위젯은 기본적으로 컨트롤 위젯이지만 현재 재생중인 음악을 사용자에게 알려주는 정보 위젯의 역할도 한다.



## Limitations of Widget

위젯의 제한사항은 다음과 같다.

### 제스쳐

위젯에서 사용할 수 있는 제스쳐는 터치와 수직 스와이프로 두가지이다.



------

# **Let's build an App Widget**



## 기본 사항

앱 위젯을 만들기 위해서는 다음이 필요하다.

- `AppWidgetProviderInfo` 객체

  앱 위젯의 레이아웃, 업데이트 빈도, AppWidgetProvider 클래스 등 앱 위젯의 메타데이터를 설명한다. XML로 정의해야 한다.

- `AppWidgetProvider` 클래스 구현

  브로드캐스트 이벤트를 기반으로 앱 위젯과 프로그래매틱 방식으로 접속할 수 있는 기본적인 방법을 정의한다. 이를 통해 앱 위젯이 업데이트, 사용 설정, 사용 중지, 삭제될 때 브로드캐스트를 수신하게 된다.

  
### 레이아웃 보기

XML로 정의된 앱 위젯의 초기 레이아웃을 정의한다.

또한 앱 위젯 구성 활동을 구현할 수 있다. 사용자가 앱 위젯을 추가할 때 실행되고 작성 시 사용자가 앱 위젯 설정을 수정할 수 있도록 하는 선택적 `Activity`이다.



## Manifest에서 앱 위젯 선언

먼저 애플리케이션의 `AndroidManifest.xml` 파일에서 `AppWidgetProvider` 클래스를 선언한다.

```xml
    <receiver android:name="DaigakuAppWidgetProvider" >
        <intent-filter>
            <action android:name="android.appwidget.action.APPWIDGET_UPDATE" />
        </intent-filter>
        <meta-data android:name="android.appwidget.provider"
                   android:resource="@xml/daigaku_appwidget_info" />
    </receiver>
```

`<receiver>` 요소에는 앱 위젯에서 사용하는 `AppWidgetProvider`를 지정하는 `android:name` 속성이 필요하다.

`<intent-filter>`요소에는 `android:name` 속성이 있는 `<action>` 요소를 포함해야 한다. 이 속성은 `AppWidgetProvider`에서 `ACTION_APPWIDGET_UPDATE` 브로드캐스트를 허용한다는 것을 지정한다.  `AppWidgetManager`는 필요한 경우 다른 모든 앱 위젯 브로드캐스트를 AppWidgetProvider로 자동 전송한다.

`<meta-data>` 요소는 `AppWidgetProviderInfo` 리소스를 지정하며 다음과 같은 속성이 필요하다.

- `android:name` - 메타데이터 이름을 지정한다. 데이터를 `AppWidgetProviderInfo` 설명자로 표시하려면 `android.appwidget.provider`를 사용한다.

- `android:resource` - `AppWidgetProviderInfo` 리소스 위치를 지정한다.

  

## AppWidgetProviderInfo 메타데이터 추가

`AppWidgetProviderInfo`는 최소 레이아웃 크기, 초기 레이아웃 리소스, 앱 위젯을 업데이트하는 빈도, 작성 시 실행할 구성 활동(선택사항) 등 앱 위젯의 기본적인 특성을 정의한다. XML 리소스에서 단일 `<appwidget-provider>` 요소를 사용하여 AppWidgetProviderInfo 객체를 정의하고 프로젝트의 `res/xml/` 폴더에 저장한다.

```xml
    <appwidget-provider xmlns:android="http://schemas.android.com/apk/res/android"
        android:minWidth="40dp"
        android:minHeight="40dp"
        android:updatePeriodMillis="43200000"
        android:previewImage="@drawable/preview"
        android:initialLayout="@layout/example_appwidget"
        android:configure="com.example.android.DaigakuAppWidgetConfigure"
        android:resizeMode="horizontal|vertical"
        android:widgetCategory="home_screen">
    </appwidget-provider>
    
```

다음은 `<appwidget-provider>` 속성의 요약이다.

- `minWidth` 및 `minHeight` 속성의 값은 앱 위젯에서 기본적으로 사용하는 공간의 최소 크기를 지정한다. 기본 홈 화면은 높이와 너비가 정의된 셀의 그리드를 기반으로 창에 앱 위젯을 배치한다. 앱 위젯의 최소 너비 또는 높이 값이 셀의 크기와 일치하지 않으면 앱 위젯 크기가 가장 가까운 셀 크기로 반올림된다.

- `minResizeWidth` 및 `minResizeHeight` 속성은 앱 위젯의 절대 최소 크기를 지정한다. 앱 위젯의 크기가 이 값보다 작으면 앱 위젯을 알아볼 수 없거나 사용할 수 없다. 이러한 속성을 사용하면 사용자가 위젯의 크기를 `minWidth` 및 `minHeight` 속성에 의해 정의된 기본 위젯 크기보다 작게 조정할 수 있다.

- `updatePeriodMillis` 속성은 앱 위젯 프레임워크에서 `onUpdate()` 콜백 메서드를 호출하여 `AppWidgetProvider`에 업데이트를 요청해야 하는 빈도를 정의한다. 정의된 빈도에 따라 제시간에 정확히 실제 업데이트가 발생하지 않을 수도 있으며 배터리를 절약하기 위해 업데이트 주기를 최대한 길게 하여 한 시간에 한 번만 업데이트하는 것이 좋다. 사용자가 구성에서 빈도를 조정하도록 할 수도 있다.

- `initialLayout` 속성은 앱 위젯 레이아웃을 정의하는 레이아웃 리소스를 가리킨다.
- `configure` 속성은 사용자가 앱 위젯을 추가할 때 앱 위젯 속성을 구성하기 위해 실행할 수 있는 `Activity`를 정의한다. 이 속성은 선택사항이다.
- `previewImage` 속성은 앱 위젯이 구성된 후 사용자가 앱 위젯을 선택할 때 표시되는 앱 위젯의 미리보기를 지정한다. 미리보기가 제공되지 않은 경우 애플리케이션의 런처 아이콘이 대신 표시된다. 이 필드는 `AndroidManifest.xml` 파일에 있는 `<receiver>` 요소의 `android:previewImage` 속성에 해당한다.
- `autoAdvanceViewId` 속성은 위젯의 호스트에서 자동으로 진행해야 하는 앱 위젯 하위 뷰의 뷰 ID를 지정한다.
- `resizeMode` 속성은 위젯의 크기를 조절할 수 있는 규칙을 지정한다. 이 속성을 사용하여 홈 화면 위젯의 크기를 가로, 세로 또는 두 축 모두의 방향으로 조절 가능하도록 만들 수 있다. 사용자는 위젯을 길게 터치하여 크기 조절 핸들을 표시한 다음 가로 및 세로 핸들을 드래그하여 레이아웃 그리드에서 크기를 변경한다. `resizeMode` 속성 값에는 'horizontal', 'vertical', 'none'이 포함된나. 위젯의 크기를 가로 및 세로로 조절 가능하도록 선언하려면 값을 'horizontal|vertical'로 지정한다.
- `minResizeHeight` 속성은 위젯의 크기를 조절할 수 있는 최소 높이를 dps 단위로 지정한다. 이 필드는 `minHeight`보다 크거나 세로 크기 조절이 사용 설정되지 않은 경우 효과가 없다.
- `minResizeWidth `속성은 위젯의 크기를 조절할 수 있는 최소 너비를 dps 단위로 지정한다. 이 필드는 `minWidth`보다 크거나 가로 크기 조절이 사용 설정되지 않은 경우 효과가 없다.
- `widgetCategory` 속성은 앱 위젯을 홈 화면(`home_screen`), 잠금 화면(`keyguard`) 또는 둘 다에 표시할 수 있는지 여부를 선언한다.