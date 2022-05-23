# TAXI_MMD2022
Klaytn javascript native boilerplate
-------------------------------------------------------------------
# settings.js
설정값을 담고있는 js 파일입니다.

DEBUG : 자동로그인 여부
PRIVATE_KEY : 본인의 클레이튼 PRIVATE_KEY
-------------------------------------------------------------------
# session.js
기본적인 블록체인기능 및 로그인 처리를 관리하는 js 파일입니다.
로그인정보를 sessionStorage를 통해 계속 유지합니다.

init : 초기화 함수. 세션스토리지에 로그인이력이 있으면, 해당 정보로 로그인

auth 
    accessType: 'keystore' or 'privateKey',
    keystore: '', // accessType이 keystore일때 사용할 키스토어 파일의 위치
    password: '', // accessType이 keystore일때 사용할 비밀번호
    wallet_address: '' // 로그인된 지갑주소 (나중에 스마트컨트랙트에서, get~~ 함수들을 호출할때, caller 파라미터에 넘겨줄 값)

handleLogin : 로그인 함수 ( 키스토어 방식과 private_key 방식 두가지가 존재.)
handleLogout : 로그아웃 함수
---------------------------------------------------------------------
# user.js
간단히 유저정보를 받아오는 js파일입니다.
블록체인 명령어 호출관련 코드를 작성할때, get~~~ 관련 명령어들은, 해당 파일을 참조해서 해도 좋을것 같아요.
-------------------------------------------------------------------------------------------------
# 블록체인 명령어 사용법

명령어는 크게 get / set으로 나뉩니다.
get : 정보를 얻어오는 코드들입니다. (가스비 X)
set : add ~~ 혹은 set ~~ 으로 정의되어있거나, checkIn/checkOut 같이, 스마트계약서 내의 정보를 "변경" 하는 명령어들입니다. (가스비 O)

get함수에서는 호출하는 자신이 누구인지 밝혀야할 필요가 있어서, 지갑주소를 항상 넘겨줘야 합니다.
set함수에서는 send()를 이용하여 보내기때문에, 자신이 누구인지 밝혀져 있습니다.(msg.sender 존재) 그래서 지갑주소를 따로 넘겨줄 필요가 없습니다.

[get 관련 함수 사용법]
agContract.methods.명령어(Session.auth.wallet_address).call()

[set 관련 함수 사용법]
agCotract.methods.명령어(parmaeters.....).send({from : Session.auth.wallet_address, gas:'200000'})  // 20만가스까지 제한둠.
--------------------------------------------------------------------------------------------------
# 블록체인 event 종류
event addReservEvent()
    - 요청목록페이지에서 나중에 쓸 이벤트입니다. 새로운 예약이 블록체인에 등록될때, 발생됩니다.

event checkInTaxiEvent(address indexed addr);
    - 택시탑승 이벤트입니다.

event checkOutTaxiEvent(address indexed addr);
    - 택시도착 이벤트입니다.

event cancelLastReservEvent(address indexed addr);
    - 예약 취소 이벤트입니다.

event payCompleteEvent(address indexed addr);
    - 결제 완료 이벤트입니다.
--------------------------------------------------------------------------------------------------
# 콜백 설정시, once, on.

once : 단 한번 콜백함수를 호출합니다. event A 발생 -> 콜백호출.   다음에 event A 발생 -> 콜백 호출 X (종료)
on : 같은 이벤트에 대해서 계속 호출합니다. event A 발생 -> 콜백호출.   다음에 event A 발생 -> 콜백 호출.
--------------------------------------------------------------------------------------
# 가스비
1gas = 750 ston = 750 * 10^9 peb
10만 가스 = 0.0075KLAY,

1KLAY = 1000원 가정시, 75원임. 비싸다.
--------------------------------------------------------------------
VSCODE 단축키

Alt+Shift+아래방향키 -> 현재줄 복사해서, 아래줄에 붙여넣기