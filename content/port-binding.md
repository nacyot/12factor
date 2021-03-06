## 7. 포트 바인딩
### 포트 바인딩으로 서비스 공개하기

웹 어플리케이션은 웹 서버 컨테이너 안에서 실행되기도 한다.  예를 들어 PHP 어플리케이션은 [Apache HTTPD](httpd.apache.org/) 내부의 모듈로 실행될 수 있으며, Java 어플리케이션은 [Tomcat](http://tomcat.apache.org/) 내부에서 실행되기도 한다.

**Twelve-Factor App은 스스로 완결된다.** 달리 말해 웹에 공개되는 서비스를 만들 때, 실행 환경에서 어플리케이션 실행하고자 웹서버를 직접적으로 사용하지 않는다.  웹 어플리케이션은 **포트 바인딩을 통해서 HTTP를 서비스로 공개하며** 이 포트로 들어오는 요청을 기다린다.

로컬 개발환경에서 개발자는 어플리케이션에 의해 공개된 서비스에 접근하기 위해 `http://localhost:5000`와 서비스 URL을 사용한다.  프로덕션 배포에서는 라우트 계층을 추가해 외부 공개된 호스트 이름에 대한 요청을 내부 포트에 바인딩된 웹 프로세스에 보내준다(route).

이를 구현하기 위해 일반적으로 [의존성 선언](/dependencies) 시에 웹 서버 라이브리러를 어플리케이션에 추가한다. 웹 서버 라이브러리의 예로는 Python의 [Tornado](http://www.tornadoweb.org/), 루비의 [Thin](http://code.macournoyer.com/thin/), 자바나 JVM 기반 언어들의 [Jetty](http://jetty.codehaus.org/jetty/) 등이 있다. 이 구현은 완전히 *사용자 공간*에서 일어나는 일이다. 즉, 어플리케이션 코드 내에서 구현된다.  요청을 처리하기 위한 실행 환경과의 규약은 포트를 바인딩하는 것이다.

포트 바인딩을 통해 공개되는 서비스는 비단 HTTP에 국한되지 않는다.  거의 모든 서버 소프트웨어는 프로세스를 포트에 바인딩해놓고 요청을 기다린다.  예를 들자면 [ejabberd](http://www.ejabberd.im/)([XMPP](http://xmpp.org/))나 [Redis(speaking [XMPP](http://xmpp.org/)([Redis 프로토콜](http://redis.io/))도 포트 바인딩을 사용한다.

포트 바인딩에서 가장 중요한 점은 공개된 포트를 통해서 특정 어플리케이션이 다른 어플리케이션에 대한 [백엔드 서비스](/backing-services)가 된다는 점이다. 백엔드 어플리케이션은 접근가능한 URL을 가지며, 이를 이용하고자 하는 어플리케이션은 [설정](/config)에 해당하는 백엔드 서비스의 리소스 핸들을 저장하기만 하면 된다.
