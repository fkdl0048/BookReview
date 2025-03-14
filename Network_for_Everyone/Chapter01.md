## 1장: 네트워크 첫걸음

네트워크란, 꼭 컴퓨터 간의 연결만을 의미하는 것은 아니다. 사람과 사람의 네트웤, 도로와 철도의 네트워크, 물류 네트워크와 같이 다양한 네트워크가 있다.

컴퓨터 네트워크는 서로 필요한 정보를 주고 받을 수 있다. 인터넷이란, 전 세계의 큰 네트워크부터 작은 네트워크까지를 연결하는 거대한 네트워크이다.

### 패킷이란?

네트워크나 인터넷에서 데이터를 주고받기 위해선 규칙이 있어야 한다. 그 규칙에는 패킷을 사용한다. 네트워크를 통해 전송되는 데이터의 작은 조각을 말하며 큰 데이터가 있더라도 작게 나누어서 보내는 것이 규칙이다.

큰 데이터를 작게 나눠서 보내는 이유는 데이터가 **대역폭**을 너무 많이 차지해서 다른 패킷의 흐름을 막을 위험이 있기 때문에 작게 나눠서 보내는 것이다. 일종의 도로의 교통상황으로 차의 크기를 제한하는 이유와 같다.

*대역폭이란, 일반적으로 네트워크에서 이용 가능한 최대 전송 속도로 정보를 전송할 수 있는 단위 시간당 전송량을 말한다.*

사진 데이터도 마찬가지로 네트워크를 통해 전송할 때도 패킷으로 작게 분할하고 이후 원래 사진대로 되돌리는 작업이 필요하다. 목적지로 보낸 패킷이 전송한 순서대로 도착하지 않을 수 있다. 또한 패킷이 전송될 때 네트워크가 지연되어서 늦게 도착하거나 누락되기도 한다.

따라서 송신하는 측에서 패킷을 보낼 때는 각 패킷에 순서대로 번호를 붙여서 보낸다. 그럼 번호에 맞춰 정렬하면 되니까 늦게 도착한 패킷도 원래 위치로 돌아갈 수 있다.

### 비트와 바이트란?

디지털 데이터란 0과 1의 집합을 의미한다. 여기서 0과 1의 정보를 나타내는 최소 단위를 비트(bit)라고 한다. 8비트가 모이면 1바이트(byte)가 된다.

아스키코드란 알파벳, 숫자, 기호 등을 다룰 수 있는 기본적인 문자 코드를 말한다. 각각 대응되는 표가 있으며 이를 통해 문자를 숫자로 변환할 수 있다. 문자도 사진과 마찬가지로 상대방에게 이 숫자를 패킷으로 나누어서 보내면 받은 쪽에서 패킷을 원래 값으로 되돌린다. 따라서 문자 데이터도 패킷으로 나누어서 네트워크에 전송하면 된다.

하지만 네트워크에 데이터를 전송하는 경우에는 비트 정보를 전기 신호로 변환하기 때문에 실제로는 네트워크에 **전기 신호**가 전송되고 있다.

### 랜과 왠

네트워크에는 랜(LAN)과 왠(WAN)이 있다. 랜은 좁은 범위의 네트워크을 말하며, 왠은 넓은 범위의 네트워크를 말한다.

랜은 특정 지역을 범위로 하는 네트워크를 말한다. 가정이나 같은 건물과 같이 지리적으로 제한된 곳에서 컴퓨터와 프린트를 연결하는 네트워크를 말한다.

왠은 지리적으로 넓은 범위에 구축된 네트워크를 말한다. 인터넷 서비스 제공자(ISP)가 제공하는 서비스를 이용하여 네트워크를 구축하는 것을 말한다. 여기서 서비스 제공자는 KT, SK브로드밴드, LG유플러스 등을 말한다.

랜의 경우엔 범위가 좁기 때문에 신호가 약해지거나 오류가 발생할 확률이 현저히 적다. 반면 왠은 거리가 멀기 때문에 오류가 발생할 확률이 더 높다. 또한 거리ㅔ 따라 속도가 떨이지기 때문에 왠은 랜보다 속도가 느리다.

### 가정에서의 네트워크 구성

네트워크는 앞에서 다룬 랜과 왠으로 크게 나눌 수 있는데, 가정에서 사용하는 네트워크는 랜이다. 인터넷을 사용하기 위해선 가장 먼저 인터넷 서비스 제공자를 결정해야 한다. 이후로 인터넷 회신을 결정해야 하는데 요즘은 대부분 광랜을 사용한다.

이후에 인터넷 서비스 제공자와 네트워크를 연결하기 위해선 공유기라는 장비가 필요하다. 영어로는 broadband router라고 한다. 이 공유기는 인터넷 서비스 제공자로부터 받은 신호를 컴퓨터나 스마트폰 등 다양한 기기에 전달해주는 역할을 한다.

정리하자면 인터넷 서비스 제공자 -> 광랜 -> 공유기 -> 컴퓨터, 스마트폰 등의 기기로 연결된다. 이 때 연결방식은 크게 유선과 무선으로 나뉜다.

### 소규모 회사에서의 네트워크 구성

가정에서의 랜 구성과 가장 큰 차이점은 DMZ(Demilitarized Zone)라는 영역이 있다. 이는 외부에 공개하기 위한 네트워크를 말한다. 서버를 공개하는데 주로 웹 서버, 메일 서버, DNS 서버를 공개한다. 웹 사이트를 불특정 다수의 외부 사용자에게 공개하려면 웹 서버를 외부에 공개하고, 외부 사용자와 메일을 주고받으려면 메일서버를 외부로 공개하고, 외부에서 도메인 이름을 이용해 웹 사이트에 접속하려면 DNS 서버를 외부로 공개한다.

회사에서 서버를 운영하기 위해 서버를 사내에 설치하거나 **데이터 센터**에 두거나 **클라우드**에 둘 수 있다. 여기서 사내 또는 데이터 센터에 서버를 두고 운영하는 것을 **온프레미스(on-premise)**라고 한다.

### 용어 정리

- 네트워크(Network): 컴퓨터를 두 대 이상 연결하여 서로 데이터를 전송할 수 있는 통신망이다.
- 인터넷(Internet): TCP/IP 프로토콜을 사용하는 세계 최대 규모의 네트워크다. 전 세계의 컴퓨터가 서로 연결하여 정보를 교환할 수 있도록 만든 하나의 거대한 컴퓨터 통신망이다.
- 패킷(Packet): 네트워크를 통해 전송되는 데이터의 작은 조각을 말한다. 네트워크에서 전송하는 데이터의 기본 단위다.
- 비트(Bit): 0과 1의 정보를 나타내는 최소 단위다.
- 바이트(Byte): 8비트를 모아 만든 것으로 컴퓨터의 정보량 단위다.
- 랜(LAN): Local Area Network의 약자로, 가까운 거리에 있는 컴퓨터를 연결한 네트워크다.
- 왠(WAN): Wide Area Network의 약자로, 넓은 범위에 있는 컴퓨터를 연결한 네트워크다.
- 인터넷 서비스 제공자(Internet Service Provider, ISP): 인터넷 서비스를 제공하는 업체다.
- 서버(Server): 컴퓨터 네트워크에서 다른 컴퓨터에 서비스를 제공하기 위한 컴퓨터 또는 프로그램이다. 반대로 서버에서 보내 주는 정보 서비스를 받는 측 또는 요구하는 측의 컴퓨터 또는 프로그램을 클라이언트라고 한다.
- DMZ(Demilitarized Zone): 외부에 공개하기 위한 네트워크다. 주로 웹 서버, 메일 서버, DNS 서버를 공개한다.

### 느낀점

간단하면서도 놓치고 있던 내용도 있어서 가볍게 읽기 좋았다. 나중에 게임서버 프로그래밍을 배우기전 아주 기초지식을 공부하는 듯함