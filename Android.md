# 2020-05-03

`` This version of Android Studio cannot open this project, please retry with Android Studio ~ or newer``
 
##### -> project build gradle에 가서 그래들 버젼을 고쳐준다
 
 
 
# 2020-05-04


`` lazy ``

##### -> lazy는 선언 해두고 실제 사용할때 초기화 하는 방법이다 


## Realm

##### Realm을 앱에서 등록(초기화)를 해 두면 어느 액티비티에서 사용하던간에 Realm 객체만 생성하면 공유가 된다.
      

##### [0]  Realm에서 사용할 모델을 먼저 만든다 만들고 난 뒤 RealmObject를 상속해야한다.
##### [1]  Realm.init으로 먼저 초기화를 해 준다. 
#####      (Application 클래스 하나 생성하고 그곳에 초기화 한 후, Manifest에 name을 Application Class 명을 적어넣는다.
##### [2]  Realm 오브젝트를 하나 만들어 주고 [ Realm.getDeaultInstance() ] 
#####      executeTransactionAsync (비동기) 문장 안에insert, update, 등을 사용한다.
  
   
```  
     realm.beginTransaction() // 트랜잭션 시작
     val managedDog = realm.copyToRealm(dog) // Persist unmanaged objects
     val person = realm.createObject<Person>(0) // Create managed objects directly
     person.dogs.add(managedDog)
     realm.commitTransaction()  // 커밋으로 적용, 트랜잭션 종료
```

##### executeTransaction는 시작과 커밋 종료를 자동으로 해준다.
##### 데이터 삽입 시 insert(모델객체)하면 끝이다




# 2020-05-05

`` 상속과 구현의 차이 (extends, implementaion) ``

 하나의 클래스를 인간이라고 봤을때
##### 인간은 동물이다 -> 상속 (근원)
##### 인간은 수리공이다 -> 구현 (역할) 


# 2020-05-09

``` Realm은 모델이 추가되거나 모델의 구조가 변경될 경우에 항상 [마이그레이션]이란 것을 해줘야 한다. ```

Realm을 Application 에서 초기화 해줄때 config를 미리 구현하고 
```
 Realm.init(applicationContext)
 val config = RealmConfiguration.Builder().deleteRealmIfMigrationNeeded().build()
 Realm.setDefaultConfiguration(config)     
```

이렇게 해주면 getDefaultInstance로 객체를 가져와도 config 에 설정해놓은 값대로 설정된다.



# 2020-05-11

``` 문자열.split(구분할문자). ```
 는 문자열을 구분할 문자별로 나눠서 배열형태로 반환해준다 참말로 좋은기능이다.
 
``` 리사이클러뷰 어댑터 매개변수로 넣는다고 무조건 매개변수 만큼의 뷰들이 생기는것이 아니다.``` 
리사이클러뷰를 움직이면 변수 position이 변경되는데 이 값에 따라서 onBind가 실행됐었다.
 
 
Realm에는 ArrayList 가 들어갈 수 없다.


# 2020-05-12


1. 생명주기에 ExoPlayer 등록
2. player 인스턴스 생성
3. PlayerView에 Player 인스턴스 등록 (setPlayer)
4. Uri를 이용해서 미디어 소스를 생성 
5. Player.prepare로 생성한 미디어 소스를 재생

### 프로그래머스 알고리즘 문제 (스택/큐) 탑 문제

```
class Solution {
    fun solution(heights: IntArray): IntArray {
        var answer = intArrayOf()
        var stack: List<Int> = listOf()
        for ((i, h) in heights.withIndex()) {
            stack = stack.dropLastWhile {t -> heights[t] <= h }
            answer +=  (stack.lastOrNull() ?: -1) + 1
            stack += i
        }
        return answer
    }
}
```
* for 안에 (i,h) index와 value 이다.
* dropLastWhile - > 마지막 element 부터 predicate 와 매칭된 결과를 제외하고 가져온다.
* stack.lastOrNull() 마지막 인자를 반환하며, 없을 경우에 Null 리턴

첩첩산중이다


# 2020-05-14

```android:configChanges```

액티비티가 처리하는 구성 변경을 나열합니다. 구성 변경이 런타임에 발생하는 경우 기본적으로 액티비티가 종료되었다가 

다시 시작하지만 이 특성을 사용하여 구성을 선언하면 액티비티가 다시 시작하는 것을 방지합니다. 

그 대신 액티비티는 여전히 실행 상태에 있고 onConfigurationChanged() 메서드가 호출됩니다.

->ExoPlayer 화면 전환시 자꾸 플레이어가 초기화 되는 현상을 이 방법으로 해결
하지만 안드로이드 공식 문서에서는 [최후의 수단]으로 사용하라고 당부하고 있음

 onSaveInstanceState(), ViewModel 등 의 기능을 이용해서 런타임 변경 처리를 할것을 권장한다.


# 2020-05-14

[ ?. ] Null Safety 연산자 -> 참조연산자를 실행하기 전에 객체가 null인지 확인부터 하고 Null이면 실행하지 않는다.

[ ?: ] elvis 연산자       -> 객체가 Null이 아니라면 그대로 사용하지만 Null 이라면 연산자 우측에 있는 객체로 대체된다.

[ !! ] Nonnull 연산자     -> 참조연산자 사용할때 null 여부를 확인하지 않도록 한다.


# 2020-05-21

ActionBar -> 뷰가 아님 액티비티 기본구성요소중 하나
ToolBar -> 뷰처럼 자유자재로 컨트롤 가능한 앱바

액션바를 만들때 많이 헷갈린 부분중 하나이다
xml파일에서 버튼을 넣어주고 하는줄 알았는데 그 방법이 아니였다.

```
setSupportActionBar(toolbar)
supportActionBar?.setDisplayHomeAsUpEnabled(true)
supportActionBar?.setHomeAsUpIndicator(R.drawable.ic_menu_black_24dp);
supportActionBar?.setDisplayShowTitleEnabled(false)
```

# 2020. 06. 15



- android : name 태그 (XML) 

  

  ```xml
  android:name="androidx.navigation.fragment.NavHostFragment"
  ```

 

  어플리케이션을 위해 구현된 어플리케이션 서브클래스의 이름. 

  어플리케이션 프로세스가 시작될 때, 어플리케이션의 다른 어떤 컴포넌트보다 먼저 객체화된다(실행된다). 서브클래스는 없어도 된다(대부분의 어플리케이션은 서브클래스를 사용하지 않는다). 서브클래스가 없으면, 안드로이드는 베이스 어플리케이션 클래스의 객체를 사용한다.

  