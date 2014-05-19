## 4. 백엔드 서비스(Backing Services)
### 백엔드 서비스를 어플리케이션에 붙여진 리소스로 다루기

백엔드 서비스란 어플리케이션이 실행되는 가운데 네트워크를 통해서 사용할 수 있는 모든 서비스를 일컫는다.  예를 들어 [Mysql](http://dev.mysql.com/)이나 [CouchDB](http://couchdb.apache.org)와 같은 데이터베이스, [RabbitMQ](http://www.rabbitmq.com)나 [Beanstalkd](http://kr.github.com/beanstalkd/)와 같은 메시지/큐 시스템, [Postfix](http://www.postfix.org)와 같은 메일 송신을 위한 SMTP 서비스, [Memcached](http://memcached.org)와 같은 캐시 시스템이 있다.

전통적으로 데이터베이스 같은 백엔드 서비스를 관리하는 사람과 어플리케이션을 배포하는 시스템 관리자는 같은 사람이었다.  이러한 로컬에서 관리되는 서비스뿐만 아니라, 서드 파티에서 제공하고 관리되는 서비스들도 있다.  예를 들어 [Postmark](http://postmarkapp.com/)와 같은 SMTP 서비스, [New Relic](http://newrelic.com)이나 [Loggly](http://www.loggly.com)와 같은 메트릭스 수집 서비스, [Amazon S3](http://aws.amazon.com/s3)와 같은 바이너리 에셋 서비스, [Twitter](http://dev.twitter.com)나 [Google Maps](http://code.google.com/apis/maps/index.html), [Last.fm](http://www.last.fm/api)과 같이 API로 접근가능한 서비스들이 있다.

** Twelve-Factor App의 코드는 로컬 서비스와 써드 파티 서비스의 차이를 구분하지 않는다.** 어플리케이션 입장에서 보자면 로컬 서비스건 써드파티 서비스건 서비스들은 [설정](/config)에 저장된 URL이나 추가적인 정보들을 통해서 접근 가능한 붙여진 리소스일 뿐이다.  Twelve-Factor App의 [배포](/codebase)에서는 어플리케이션 코드의 변경 없이도 로컬 Mysql 데이터베이스 서비스를 써드파티 서비스(예를 들어인 [Amazon RDS](http://aws.amazon.com/rds/))로 대체할 수 있어야한다.  마찬가지로 로컬에서 관리되는 SMTP 서버도 코드 변경없이 Postmark 같은 써드파티 SMTP  서비스로 대체할 수 있어야한다.  두 경우 모두 변경이 필요한 부분은 설정의 리소스 핸들 뿐이다.

각각의 벡엔드 서비스는 *리소스*이다.  예를 들어 Mysql 데이터베이스는 하나의 리소스이다. 하지만 어플리케이션 계층에서 샤딩으로 사용되는 두 개의 Mysql 데이터베이스들은 두개의 다른 리소스라고 할 수 있다.  Twelve-Factor App은 이러한 데이터베이스들을 *붙여진 리소스*로 다룬다. 이는 붙여진 리소스와 리소스가 붙여지게될 어플리케이션의 배포가 느슨하게 결합되어있다는 것을 의미한다.

<img src="/images/attached-resources.png" class="full" alt="A production deploy attached to four backing services." />

리소스는 언제든지 붙이거나 떼어낼 수 있다.  예를 들어 하드웨어에 문제가 발생해 어플리케이션이 정상적으로 작동하지 않는다면 관리자는 백업 파일로부터 새로운 데이터베이스 서버를 실행시킬 것이다.  데이터베이스가 실행되면 기존의 프로덕션 데이터베이스를 떼어내고(detach), 새로운 데이터베이스를 어플리케이션에 붙인다(attach). 이 과정에는 어떠한 코드 수정도 없다.

