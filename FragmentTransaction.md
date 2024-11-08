# FragmentTansaction
- 프래그먼트를 추가, 제거, 교체, 숨기기 또는 표시하는 작업을 수행하는 객체
- 작업을 하나의 단위로 묶어 원자성을 보장

## 특징
|Title|Description|
| - | - |
| 프래그먼트 관리 및 UI 구성 | - FragmentTransaction을 사용하면 액티비티 내에서 여러 프래그먼트 효율적으로 관리하여 동적인 UI 구정이 가능<br/>- 사용자가 상호작용에 따라 프래그먼트 교체하거나, 특정 프래그먼트를 숨기는 식으로 UI를 유역하게 조정할 수 있음 |
| 트랜잭션의 원자성 | - 트랜잭션은 하나의 단위로 실행되므로, 트랜잭션 중 일부 작업만 실행되는 일이 없도록 보장<br/>- 트랜잭션 내에서 오류가 발생하거나 트랜잭션이 실패하면 이전 상태로 돌아가 UI의 일관성을 유지함|
| 백스택 | - 프래그먼트를 백스택에 추가하여 '뒤로 가기' 기능을 통해 이전 상태로 돌아갈 수 있도록 함<br/>- addToBackStack() 메서드를 사용하면 트랜잭션을 백스택에 추가하여 프래그먼트 간의 탐색을 원할하게 만듦 |

## 트랜잭션 커밋
- commit()
  - 비동기적으로 프래그먼트 트랜잭션을 실행 -> 트랜잭션이 즉시 실행되지 않고 다음 메인 스레드의 루프에서 실행됨
  - 성능 저하를 최소화할 수 있음
  - 트랜잭션이 즉시 실행될 필요가 없고, UI 업데이트를 안전하게 비동기적으로 처리하고 싶을 때 적합
- commitNow()
  - 트랜잭션을 즉시 실행 -> 현재 스레드에서 바로 트랜잭션을 완료하며, 프래그먼트가 즉각적으로 반영됨
  - 백 스택에 추가할 수 없으며, addToBackStack()과 함계 사용할 경우 오류 발생
  - 트랜잭션이 즉시 반영되어야 하며, 백 스택에 추가할 필요가 없는 경우 사용
- commitAllowingStateLoss()
  - commit()과 유사하게 트랜잭션을 비동기적으로 실행하지만, 상태 손실을 허용함
  - ex) 액티비티의 상태가 이미 저장된 경우와 같이, 시스템이 트랜잭션을 실행 중에도 오류를 무시하도록 허용
  - 상태 손실을 허용하기 때문에, 사용자 경험에 민감한 UI 업데이트에는 사용하지 않는 것이 좋음
  - 트랜잭션이 반드시 실행되어야 하지만, UI 상태가 이미 저장되어 잠재적인 손실을 감수할 수 있는 상황에 사용
- commitNowAllowingStateLoss()
  - commitNow()와 유사하지만 상태 손실을 허용. 즉시 트랜잭션을 실행하면서, 상태 손실을 허용하는 점에서 차이가 있음
  - 시 반영이 필요하고 상태 손실을 감수할 수 있을 때 사용
- executePendingTransactions()
  - 현재 대기 중인 모든 프래그먼트 트랜잭션을 즉시 실행
  - 새로운 트랜잭션을 추가하지 않고, 대기 중인 트랜잭션만 실행하는 점이 차이점
  - 특정 시점에 대기 중인 트랜잭션을 실행해야 하는 상황에서 사용

## commitNow vs executePendingTransactions

||commitNow|executePendingTransactions|
| - | - | - |
| 동작 방식 | - 새로운 트랜잭션을 생성하고, 해당 트랜잭션을 즉시 실행하여 UI에 반영 | - 이미 커밋된 트랜잭션들 중 대기 중인 것들을 즉시 실행<br/>- 새로운 트랙잭션을 생성하지는 않고 단지 대기 상태에 있는 트랜잭션을 처리|
| 제약 사항 | - 백스택에 추가할 수 없기 때문에 addToBackStack()과 함께 사용하면 예외 발생| - 대기 중인 프래그먼트가 없으면 그냥 리턴함|
| 사용 상황 | - 새 트랜잭션을 즉각적으로 실행하고 반영할 필요가 있을 때 사용| - 특정 시점에 대기 중인 트랜잭션을 즉시 실행해야 하는 경우에 사용|

## commitAllowingStateLoss vs commitNowAllowingStateLoss

||commitAllowingStateLoss|commitNowAllowingStateLoss|
| - | - | - |
| 동작 방식 | - 동기적으로 트랜잭션을 실행. 트랜잭션은 다음 메인 스레드 루프에서 실행되며, 즉시 반영되지 않음 | - 즉시 트랜잭션을 실행. 이 메서드는 트랜잭션을 현재 스레드에서 바로 반영하며, UI에 바로 적용함|
| 제약 사항 | - 상태 손실 위험| - 백 스택에 추가할 수 없기 때문에 addToBackStack()과 함께 사용할 수 없음|
| 사용 상황 | - 트랜잭션을 상태 손실 위험이 있더라도 안전하게 비동기적으로 처리하고자 할 때 사용| - 트랜잭션이 즉각적으로 UI에 반영되어야 할 때 사용|
