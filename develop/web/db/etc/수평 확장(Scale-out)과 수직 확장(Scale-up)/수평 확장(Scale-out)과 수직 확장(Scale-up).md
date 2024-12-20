# 수평 확장(Scale-out)과 수직 확장(Scale-up)

- 수평 확장(Scale-out)과 수직 확장(Scale-up)은 시스템 성능을 개선하고 확장하기 위한 두 가지 주요 방법이다.
- 데이터베이스나 서버 인프라에서 더 많은 데이터를 처리하거나 더 많은 트래픽을 감당하기 위해 이 두 가지 방식이 사용된다.
- 각 확장 방법은 장단점이 있으며, 상황에 따라 적합한 방식이 선택된다.

---

# **수평 확장(Scale-out)**

- 수평 확장(Scale-out은 시스템에 추가적인 서버(노드)를 여러 대 배치하여 성능을 확장하는 방식이다.
- 즉, 데이터를 처리하는 여러 대의 서버를 **병렬로 연결**하여 더 많은 요청을 동시에 처리하거나, 데이터를 분산하여 저장할 수 있다.
- 클러스터링을 통해 여러 서버를 연결하고, 그 위에 애플리케이션을 배포하는 방식이다.

## **방법**:

- 서버를 여러 대 추가하여 시스템 성능을 향상시킴.

## **예시**:

- 웹 서버를 여러 대 운영하거나, 데이터베이스를 샤딩(Sharding)하여 데이터를 분산 저장하는 방식.

## 장점:

- **무한한 확장성**:
    - 이론적으로는 서버나 노드를 계속 추가할 수 있기 때문에 확장 가능성이 매우 크다. 트래픽이나 데이터 양이 증가할 때 서버를 더 추가함으로써 대응할 수 있다.
- **내결함성(Fault Tolerance)**:
    - 여러 대의 서버가 병렬로 동작하기 때문에, 한 서버가 장애를 일으켜도 다른 서버들이 계속해서 서비스를 제공할 수 있어 가용성이 높다.
- **부하 분산**:
    - 여러 서버에 트래픽을 나누어 처리할 수 있으므로, 부하를 분산시키고 성능을 극대화할 수 있다.

## 단점:

- **복잡성 증가**:
    - 여러 대의 서버를 관리하기 위한 인프라, 네트워크 설정, 데이터 분산 등 시스템이 복잡해진다.
- **데이터 일관성 문제**:
    - 여러 서버 간에 데이터를 분산하면, 데이터 일관성을 유지하는 것이 어려워질 수 있다. 이를 해결하기 위해 분산 시스템에 맞는 데이터 일관성 모델이 필요하다.
- **초기 비용**:
    - 추가적인 하드웨어 또는 가상 서버를 구입해야 하므로 초기 비용이 증가할 수 있다.

## 사용 사례:

- **대규모 웹 애플리케이션**:
    - 많은 사용자가 동시에 접속하는 웹 서비스는 여러 대의 웹 서버, 데이터베이스 샤드 등을 통해 수평 확장을 활용한다.
- **NoSQL 데이터베이스**:
    - MongoDB, Cassandra 같은 NoSQL 시스템은 기본적으로 수평 확장을 염두에 두고 설계되었다. 데이터는 여러 노드에 분산되어 저장되고 처리된다.

---

# **수직 확장(Scale-up)**

- 수직 확장(Scale-up)은 기존의 서버에 더 많은 자원(메모리, CPU, 스토리지 등)을 추가하여 성능을 확장하는 방식이다.
- 즉, 한 서버의 **성능을 업그레이드**하는 방식으로, 서버의 하드웨어 성능을 높여 더 많은 데이터나 요청을 처리할 수 있도록 한다.

## **방법**:

- 기존 서버에 CPU, 메모리, 스토리지 등을 업그레이드하여 성능을 향상시킴.

## **예시**:

- 서버에 더 강력한 CPU, 더 많은 메모리, 빠른 스토리지 장치를 추가하거나 교체하여 성능을 높이는 것.

## 장점:

- **관리 용이성**:
    - 여러 대의 서버를 관리하는 수평 확장과 달리, 하나의 서버만 관리하면 되므로 운영이 상대적으로 간단하다.
- **데이터 일관성**:
    - 모든 데이터가 한 서버에 저장되고 처리되므로, 데이터 일관성을 유지하는 것이 상대적으로 쉬운 편이다.
- **기존 시스템 활용**:
    - 기존 시스템을 그대로 유지하면서 업그레이드만 진행하므로, 새로운 아키텍처나 분산 시스템에 대한 학습과 구현이 필요 없다.

## 단점:

- **확장성의 한계**:
    - 하드웨어 성능에는 물리적인 한계가 있으며, 일정 수준 이상으로는 더 이상 확장할 수 없다. 예를 들어, 서버에 추가할 수 있는 CPU나 메모리 양은 제한적이다.
- **단일 장애 지점(Single Point of Failure)**:
    - 하나의 서버에 모든 데이터를 저장하고 처리하므로, 해당 서버에 문제가 발생하면 시스템 전체가 중단될 수 있다. 즉, 가용성이 떨어진다.
- **비용**:
    - 고성능 하드웨어는 비용이 많이 들 수 있으며, 같은 성능을 제공하기 위해서는 수평 확장보다 비용 효율이 떨어질 수 있다.

## 사용 사례:

- **소규모 시스템**:
    - 트래픽이나 데이터 처리 요구가 크지 않은 시스템에서는 서버 한 대만으로 충분히 성능을 충족시킬 수 있다.
- **전통적인 관계형 데이터베이스**:
    - 대부분의 RDBMS는 수직 확장 방식으로 성능을 높이는 것이 일반적이다. 서버 성능을 강화하여 더 많은 트랜잭션과 데이터를 처리할 수 있다.

---

# 비교

| 항목 | **수평 확장 (Scale-out)** | **수직 확장 (Scale-up)** |
| --- | --- | --- |
| **확장 방식** | 서버를 추가하여 시스템을 확장 | 서버의 하드웨어 성능을 업그레이드 |
| **확장성** | 이론적으로 무한한 확장 가능 | 하드웨어 성능의 한계로 확장성 제한 |
| **비용** | 초기 비용 높음 (서버 추가) | 고성능 하드웨어의 비용이 높을 수 있음 |
| **관리 복잡성** | 서버 간 분산 관리 필요 | 하나의 서버만 관리하므로 상대적으로 간단 |
| **가용성** | 서버 장애 시 다른 서버에서 서비스 가능 | 단일 서버 장애 시 전체 서비스 중단 가능 |
| **부하 분산** | 여러 서버에 트래픽을 분산하여 처리 | 한 서버가 모든 부하를 처리해야 함 |
| **데이터 일관성** | 데이터 분산으로 일관성 관리가 어려울 수 있음 | 모든 데이터가 한 서버에 저장되어 일관성 유지 쉬움 |
| **사용 사례** | 대규모 시스템, 분산 데이터 처리, 클라우드 | 소규모 시스템, 단일 서버 기반 서비스 |

---

# 정리

- **대규모 트래픽 처리와 확장성**이 중요한 경우에는 **수평 확장**이, **간단한 관리와 일관성 유지**가 중요한 경우에는 **수직 확장**이 더 적합할 수 있다.

## **수평 확장(Scale-out)**:

- 대규모 데이터와 트래픽을 처리해야 하는 시스템에서 효과적이다.
- 서버를 여러 대 추가함으로써 성능을 향상시키고, 내결함성과 확장성을 높일 수 있다.
- 하지만 시스템 관리 복잡성 증가와 데이터 일관성 문제가 생길 수 있다.

## **수직 확장(Scale-up)**:

- 상대적으로 간단한 구조의 시스템에서 성능을 개선하는 데 효과적이며, 기존 시스템을 업그레이드하는 데 유리하다.
- 그러나 하드웨어 성능에 한계가 있고, 단일 장애 지점 문제가 존재한다.