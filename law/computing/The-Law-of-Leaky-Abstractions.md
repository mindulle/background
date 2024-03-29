### 허술한 추상화의 법칙

[The Law of Leaky Abstractions on Joel on Software](https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/)

> 모든 사소하지 않은 추상화는, 어느 정도, 허술하다.
>
> —조엘 스폴스키

이 법칙에 따르면 컴퓨팅에서 복잡한 시스템과 작업하기 위해 일반적으로 사용되는 추상화에서는 때에 따라 하단 요소의 '누수'가 일어날 수 있고, 이는 추상화가 예상치 못한 방향으로 전개되게 한다.

파일을 불러와 내용을 읽는 상황을 살펴보자. 파일 시스템 API는 로우 레벨 커널 시스템의 _추상화_ 인데, 이 시스템 또한 자기 플래터(혹은 플래시 메모리나 SSD)의 데이터를 물리적으로 변경하는 것의 추상화이다. 대부분의 경우 파일을 이진 데이터의 스트림으로서 추상화하는 것은 문제가 없을 것이다. 그러나 자기 디스크에서는 데이터를 순서대로 읽는 것이 임의 접근보다 *비교할 수 없을 만큼* 빠른 반면에(페이지 폴트 비용 때문에), SSD에서는 전혀 상관이 없다. 내부 구현을 상세히 이해하여야 이런 경우에 대처할 수 있는데(가령, 데이터베이스 인덱스 파일은 임의 접근의 비용을 줄이도록 구조가 짜여져 있다), 이렇듯 추상화는 개발자가 모르면 곤란하도록 내부 구현 상세를 '누출'한다.

위의 예시는 _더 많은_ 추상화가 도입되면 더욱 복잡해질 수 있다. 리눅스 운영체제는 네트워크를 경유하여 파일에 접근할 수 있도록 하면서도 로컬에서는 '일반' 파일로 취급한다. 이런 추상화는 네트워크 오류가 발생하면 '허술해질' 것이다. 만약 개발자가 이런 파일들을 네트워크 지연이나 오류에 대한 고려 없이 '일반' 파일로 취급한다면, 버그가 생길 것이다.

이 법칙을 설명하는 글에 따르면 하부 구동 원리를 모른 채로 추상화에 과도하게 의존할 경우, 도리어 문제 해결을 복잡하게 만들 수 있다고 하고 있다.

#### 참고 :
- [[Hyrums-Law-The-Law-of-Implicit-Interfaces|하이럼의 법칙]]

#### 실제 사례:
- [포토샵의 느린 초기 로딩](https://forums.adobe.com/thread/376152) - 과거에 마주한 문제이다. 포토샵은 종종 켜는 데에 몇 분씩이나 걸리기도 하는데, 이 문제는 구동 시작시에 현재 기본으로 설정된 프린터의 정보를 읽어오는 것에서 발생하였다. 만약 그 프린터가 네트워크 프린터라면 극도로 오랜 시간이 걸리게 되는 것이다. 시스템에 네트워크 프린터의 _추상화_ 가 로컬 프린터와 유사하게 제공된 점은 연결 상태가 좋지 못한 상황의 사용자에게 문제를 일으켰다.

