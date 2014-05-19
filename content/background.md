배경
==========

이 문서의 저자는 수백개 어플리케이션의 개발과 배포에 직접적으로 관여했으며, [히로쿠(Heroku)](http://www.heroku.com/) 플랫폼에서 일하면서 수천개 어플리케이션의 개발(development), 운용(operation), 확장(scaling)에 간접적으로 참여했다.

이 문서는 실무에서 다양하게 사용되는 "서비스로서의 소프트웨어" 어플리케이션들에 대한 우리의 모든 경험과 관찰을 집약한 책입니다.  이는 어플리케이션 개발의 이상적인 프렉티스에 대한 삼각측량과 같다. 여기서는 특히 시간에 따른 어플리케이션의 유기적 성장에 대한 역학, 어플리케이션의 코드베이스를 둘러썬 프로그래머 간 협업의 역학, 그리고 [소프트웨어 부패에 대한 비용 회피]에 주목하고 있다.

이 책을 집필한 동기는, 우리가 모던한 어플리케이션 개발에서 부딪혀온 특정한 종류의 시스템 문제에 대한 관심을 높이고자함이다. 나아가 이러한 문제를 논의 하기 위한 어휘를 제공하고, 이 문제에 대한 넓고 개념적인 해결책을 전문용어와 함께 제공하는 것이다.  이 책의 형식은 Martin Fowler의 서적 *[Patterns of Enterprise Application Architecture](http://books.google.com/books/about/Patterns_of_enterprise_application_archi.html?id=FyWZt5DdvFkC)*와 *[Refactoring](http://books.google.com/books/about/Refactoring.html?id=1MsETFPD3I0C)*의 영향을 받았다.

