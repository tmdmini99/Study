![[Kotlin 개념1.png]]



Activity  
- 앱의 한 화면이다  
  
- Life Cycle(수명 주기)  
- onCreate  
    -> activity가 만들어질 때 단 한번만 호출 된다.  
    -> activity를 만들 때 단 한번만 하면 되는 작업들은 여기에서 해준다.  
- onStart  
- onResume  
    -> 다시 앱으로 돌아올 때 무조건 호출된다.  
- onPause  
    -> 화면의 일부가 가려졌을 때  
- onStop  
    -> 화면 전부가 보이지 않을 때  
- onDestroy
	- 
- intent
	- intent란 messaging object(메세지 객체) 이다. 이 객체를 통해 다른 컴포넌트 간에 정보를 주고 받을 수 있다.
	- 
```kotlin
btnStart.setOnClickListener { val intent = Intent(this, QuizQuestionsActivity::class.java) startActivity(intent) }
```

this 가 현재 액티비티이며, QuizQuestionsActivity::class.java가 이동할 액티비티이다.
startActivity(intent) 를 해주므로 화면 전환 코드는 끝이 난다. 꼭 startActivity를 해줘야한다.
