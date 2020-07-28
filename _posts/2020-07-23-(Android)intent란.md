---
title: (Android) Intent 란?
tags: Android 
---
 

지난 포스트에서 안드로이드 컴포넌트에 대하여 알아보았습니다. 이번 포스트에서는 컴포넌트 간에 통신을 하기 위한 메시지 객체인 인텐트(Android Intent)에 대하여 알아보겠습니다. 

## 인텐트란?

인텐트란 애플리케이션 컴포넌트 간에 작업 수행을 위한 정보를 전달하는 역할을 하며, 쉽게 말해 통신수단으로 보면 됩니다. 예로는 액티비티 간의 화면 전환이 있습니다. 즉, 인텐트는 컴포넌트 A가 컴포넌트 B를 호출할 때, 필요한 정보를 가지고 있으며, 이 정보에는 호출 대상이 되는 컴포넌트 B의 이름이 명시적으로 표시가 됨과 동시에 속성들이 암시적으로 표시되기도 합니다. 그리고 호출된 컴포넌트 B가 호출한 컴포넌트 A로 어떠한 결과를 전달할 때도 인텐트가 사용이 됩니다. 

- 서로 독립적으로 동작하는 4가지 컴포넌트들 간의 상호 통신을 위한 장치입니다. 
- 컴포넌트에 Action, Data, Category등을 전달합니다. 
- 인텐트를 통하여 다른 애플리케이션의 컴포넌트를 활성화시킬 수 있습니다. 

Intent를 사용하는 방법은 여러가지가 있지만 일반적으로 3가지입니다. 

- 엑티비티의 시작: startActivity, startActivityForResult
- 서비스의 시작: startService, bindService
- 브로드케스트 전달: sendBroadcast, sendOrderedBroadcast, sendStickyBroadcast 등이 있다. 
각 메서드들은 메시지를 함께 전달 할 수 있고, 호출된 컴포넌트에서 받을 수 있다.

## 인텐트 유형

### 명시적 인텐트

실행할 컴포넌트 클래스명을 지정하여 사용합니다. 명시적으로 하나의 컴포넌트를 선택하여 메시지를 전달하는 방법입니다. 
Intent intent = new Intent(context, SubActivity.class);
startActivity(intent);

### 암시적 인텐트 

특정 컴포넌트의 클래스명 없이 어떠한 작업을 수행할 것인지만 선언합니다. 해당 인텐트를 처리할 수 있는 컴포넌트를 시스템이 필터링 하여 수행하거나 사용자에게 선택하도록 합니다. 즉, 암시적으로 여러 컴포넌트 중 지정한 특성을 가진 컴포넌트 하나를 선택하여 사용하는 방법이다. Action, Category, Data를 사용해 인텐트 필터에 걸러내고 남은 컴포넌트를 찾아 이중 한가지를 createChooser()로 선택하여 실행합니다. 
여기서 암시적 인텐트를 집중해서 보자. Action, Category, Data는 뭘까? 

**Action**: 수행할 작업을 나타내는 문자열입니다. 일반적으로 Intent내에 리터럴 상수로 정의 되어있습니다. 

**Category**: 인텐트를 처리해야 하는 컴포넌트의 종류에 대한 추가 정보를 담은 문자열입니다. 대부분의 경우 카테고리가 없어도 됩니다. 

**Data**: 작업을 수행할 데이터 및 해당 데이터의 MIME type(Multipurpose Internet Mail Extensions)(홈페이지 상에 표현되는 멀티미디어 데이터에 대한 형식을 말한다.)을 참조하는 Uri객체(Uniform Resource Identifier)(인터넷에 있는 자원을 나타내는 유일한 주소) 입니다. 
데이터의 URI는 setData()를 통해, MIME type은 setType()으로 설정이 가능합니다. 

```JAVA

<intent-filter>
	                <action android:name="android.intent.action.SEND" />
	                <category android:name="android.intent.category.DEFAULT" />
	                <data android:mimeType="*/*" />
	            </intent-filter>

Intent intent = activity.getIntent();
	        String action = intent.getAction();
	        String type = intent.getType();

kakaoUri = intent.getParcelableExtra(Intent.EXTRA_STREAM);
@Override
	    public void onActivityResult(int requestCode, int resultCode, Intent data) {
…


```

프로젝트로 진행한 코드 중 일부이다. 외부 앱인 카카오톡에서 대화방 데이터를 받아오기 위해 암시적 intent를 사용하는 코드의 일부이다. 앱이 카카오톡에서 데이터를 받을 수 있도록 하기 위해서(ACTION_SEND 인텐트를 받기 위해) Manifest 파일에 <intent-filter> 엘리먼트를 추가합니다. 그리고 kakaoUri를 intent를 통해서 초기화 한 후, 이를 바탕으로 새로운 엑티비티를 띄우는 코드 입니다. 

이 외에 추가로 intent 에 포함될 수 있는 사항들은 이와 같습니다. 


- Component Name: 명시적 인텐트의 경우 시작할 컴포넌트 이름을 포함시킵니다. 
- Extras: 요청한 작업을 수행하기 위해 필요한 추가 정보를 담고 있는 Key:Value 입니다. 
- Flags: 인텐트에 대한 메타데이터 같은 기능을 합니다. 액티비티를 시작하는 방법에 대해 명시할 수도 있고 액티비티를 시작한 다음 어떻게 처리할지도 명시 할 수 있습니다. 

## 결론

Intent는 컴포넌트로 구성되어 있는 안드로이드 앱에서 중요한 전달자 역할을 합니다. 
다음 시간에는 intent 작성시 사용하는 context 에 대하여 알아보도록 하겠습니다. 







