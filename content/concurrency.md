## 8. 병행성(Concurrency)
### 스케일 아웃을 위한 프로세스 모델

모든 컴퓨터 프로그램은 한 번 실행되면 하나나 여러개의 프로세스로 보여진다.  웹 어플리케이션 역시 다양한 프로세스 실행 형태를 가진다.  예를 들어 PHP 프로세스는 Apache의 자식 프로세스로 실행되며 리퀘스트 양에 따라서 프로세스가 실행된다.  자바 프로그램은 이와 반대 방법을 사용한다. JVM은 하나의 거대한 부모 프로세스에 해당한다. 처음 실행할 때 CPU나 메모리 같은 자원들을 커다란 블록으로 확보해놓고 쓰레드를 사용해 내부적으로 병행성을 관리한다.  어느 쪽이건 실행중인 프로세스는 어플리케이션 프로그래머에게 거의 감추어져있다.

![Scale is expressed as running processes, workload diversity is expressed as process types.](/images/process-types.png)

**Twelve-Factor App에서 프로세스는 일급 시민에 해당한다.** Twelve-Factor App의 프로세스는 [서비스 데몬을 실행하기 위한 유닉스 프로세스 모델](http://adam.heroku.com/past/2011/5/9/applying_the_unix_process_model_to_web_apps/)에서 큰 영감을 받았다.  프로그래머는 이 모델을 통해 서로 다른 종류의 작업을 프로세스 타입에 따라 나누는 방식으로 어플리케이션이 다양한 종류의 작업를 처리할 수도록 설계할 수 있다.  예를 들어 HTTP 요청은 웹 프로세스가 처리하며 시간이 걸리는 백그라운드 작업은 워커 프로세스를 통해 처리한다.

이러한 모델이 런타임 VM 내의 쓰레드나 [EventMachine](http://rubyeventmachine.com/), [Twisted](http://twistedmatrix.com/trac/), [Node.js](http://nodejs.org/) 등 비동기 이벤트 모델을 사용하는 각 프로세스 내부의 다중화를 금지한다는 의미는 아니다.  하지만 각각의 VM은 어느 규모 이상으로 수직적 확장(Vertical scale)이 불가하므로 어플리케이션은 다수의 물리 머신에서 실행되는 다수의 프로세스로 확장되지 않으면 안 된다.

이 프로세스 모델의 진가는 스케일 아웃에서 발휘된다.  [아무것도 공유하지 않으며 수평 분할 가능한 Twelve-Fatro App의 프로세스](/processes)의 성질 상 병행성을 확장하는 일은 견고하면서도 간단하다.  프로세스 타입과 각 타입의 프로세스 수 배열은 *프로세스 포메이션*이라고 불린다.

Twelve-Factor App 프로세스는 [결코 데몬화되어서는 안 되며](http://dustin.github.com/2010/02/28/running-processes.html) PID 파일을 별도로 작성해서도 안 된다.  대신 운영체제의 프로세스 매니져(예를 들어 [Upstart](http://upstart.ubuntu.com)나 클라우드 플랫폼의 분산 프로세스 매니저, 개발 환경의 [Foreman](http://blog.daviddollar.org/2011/05/06/introducing-foreman.html) 등)을 사용해 [출력 스트림](/logs)을 관리하고, 프로세스의 비정상적 종료에 대응해야하며, 관리자에 의한 프로세스 재시작과 종료를 관리해야한다.
