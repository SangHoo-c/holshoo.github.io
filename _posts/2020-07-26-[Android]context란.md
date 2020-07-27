---
title: [Android] Context 란?
tags: Android 
---
 
지난 포스트 intent 에 이어, 안드로이드 개발 시 쉽게 넘어갈 수도 있지만 중요한 개념들에 대하여 알아보겠습니다. 

getApplicationContext(), this, getBaseContext(), getApplication(), 안드로이드 개발을 해본 사람이라면 수도없이 만났을 메서드 들입니다. 이들은 context와 관련된 메서드들입니다. 저 또한 앱을 만들면서 상황에 맞는 메서드를 사용했던 기억이 있습니다. 하지만 context에 대한 이해없이 무작정 번갈아가며 넣어보고 오류가 발생하면 교체하는 식의 코드를 작성했던 기억이 있습니다. 따라서 이 포스트에서는 context가 무엇이며, 어떤 종류의 context가 있는지, 그리고 언제 사용해야 하는지 알아보겠습니다. 

## Context 의 정의 
Context의 정의를 정리하면 이와 같습니다, 

-Application 환경에 대한 전역 정보를 접근하기 위한 인터페이스(인터페이스?)
-추상 클래스이며 실제 구현은 Android 시스템에 의해 제공된다. 
-Context를 통해 어플리케이션에 특화된 리소스나 클래스에 접근할 수 있다. 
-Activity 실행, Intent 브로드캐스팅 그리고 Intent 수신 등과 같은 응용 프로그램 수준의 작업을 수행하기 위한 API를 호출할 수 있다. 

조금 풀어서 적어보자면, 애플리케이션(객체)의 현재 상태의 맥락(context)를 의미합니다. 컨텍스트는 새로 생성된 객체가 지금 어떤 일이 일어나고 있는지 알 수 있도록 합니다. 따라서 액티비티와 애플리케이션에 대한 정보를 얻기 위해서는 컨텍스트를 사용하면 됩니다. 

또한 컨텍스트 (Context)는 시스템의 핸들과도 같습니다. 리소스, 데이터베이스, preference 등에 대한 접근을 제공합니다. 우리가 흔히 사용하는 컴포넌트인 액티비티 객체는 컨텍스트 객체를 상속받습니다. 따라서 액티비티는 애플리케이션의 특정 리소스와 클래스, 그리고 애플리케이션 환경에 대한 정보에 대해 접근할 수 있게 해줍니다.

이처럼 안드로이드 개발에서 컨텍스트 (Context)는 어디에나 있고, 가장 중요한 것입니다. 

시작하기에 앞서서 스택오버플로우에 올라온 질문을 보며 구체화를 해보겠습니다. 

[출처](https://stackoverflow.com/questions/10347184/difference-and-when-to-use-getapplication-getapplicationcontext-getbasecon)


```JAVA
Toast.makeText(LoginActivity.this, "LogIn successful", Toast.LENGTH_SHORT).show();
Toast.makeText(getApplication(), "LogIn successful", Toast.LENGTH_SHORT).show();
Toast.makeText(getApplicationContext(), "LogIn successful", Toast.LENGTH_SHORT).show();
Toast.makeText(getBaseContext(), "LogIn successful", Toast.LENGTH_SHORT).show();
```

```JAVA
Intent intent = new Intent(getApplicationContext(), LoginActivity.class);
Intent intent = new Intent(MenuPagina., LoginActivity.class);
Intent intent = new Intent(getBaseContext(), LoginActivity.class);
Intent intent = new Intent(getApplication(), LoginActivity.class);
```


각 코드 안에서 LoginActivity.this, getApplication(),  getApplicationContext(), getBaseContext() 의 차이점이 뭘까요?

해당 질문에 답하기 위해선 두 종류의 Context가 있다는 것을 알아야합니다.

## 애플리케이션 Context

이 경우 Context는 애플리케이션 자체의 생명주기(life cycle)와 항상 함께 합니다. 따라서 토스트 메시지와 같이 언제 어디서나 호출될수 있는 경우, 애플리케이션 Context를 사용하면 됩니다. 

## 액티비티 Context

액티비티 Context 의 경우, 액티비티의 생명주기와 함께 작동하고, onDestroy() 와 함께 사라진다. 즉, 예시에서 액티비티에 대한 환경 정보들이 Context에 있고, 만약 이 Context에 Intent를 통해 다른 액티비티를 띄우면, 액티비티 스택이 쌓이게 되는 것입니다. 따라서 새로운 액티비티가 현재의 액티비티와  연결되기 위해선 액티비티 Context를 사용해야합니다. 

추가로 애플리케이션 Context를 사용해도 됩니다. 하지만, Intent.FLAG_ACTIVITY_NEW_TASK 를 설정해줘야합니다.

**자 이제 다시 질문으로 돌아오자면,**

- LoginActivity.this : LoginActivity 는 Activity 를 상속, Activity 는 Context class를 상속하므로, LoginActivity.this 는 activity context 를 제공합니다. 즉, **this** 키워드는 activity context를 가리키는 것입니다. 

- getApplication() : Application 객체를 참조하고, Application 객체는 Context 클래스를 상속하므로, application Context 를 제공합니다. 

- getApplicationContext() : application Context 를 제공합니다. 

- getBaseContext() : activity Context 를 제공합니다. 

**정리하자면, 뷰를 수정할때는 Activity Context 나머지의 경우는 Application Context 를 사용하면 됩니다.**

## 결론 

가장 빈번하게 사용하는 Context 객체를 이제는 알고 쓸 수 있을 것 같습니다. 
Activity, Application Context  설명에 생명주기(life cycle) 이 언급되었는데 다음 포스트는 생명주기에 대해서 알아보려고 합니다. 















