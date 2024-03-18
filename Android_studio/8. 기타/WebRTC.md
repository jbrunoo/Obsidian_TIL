[sktdoves 블로그](https://getstream.io/blog/webrtc-on-android/)

#### WebRTC(Web Real-Time Communication) 
	웹 실시간 통신 기술
	웹 브라우저 상에서는 어떠한 플러그인도 필요 없이 음성 채팅, 화상 채팅, 데이터 교환까지 가능

#### WebRTC 프로토콜
	오디오 및 비디오 스트리밍, 여러 클라이언트 간에 전송되는 일반 데이터 및 직접 P2P 연결 설정과 같은 실시간 통신을 허용
	즉, 서버를 건드리지 않고 peer to peer. 실시간 클라이언트 간에 데이터를 전송할 수 있음


[medium 글](https://medium.com/@hyun.sang/webrtc-webrtc%EB%9E%80-43df68cbe511)
#### 크게 3가지의 클래스
	MediaStream - 카메라와 마이크 등의 데이터 스트림 접근
	RTCPeerConnection - 암호화 및 대역폭 관리 및 오디오, 비디오의 연결
	RTCDataChannel - 일반적인 데이터의 P2P 통신

	3가지의 객체를 통해서 데이터 교환이 이루어지고 RTCPeerConnection들이 적절하게 데이터를 교환할 수 있게 처리해주는 과정을 시그널링(Signalling)
	연결을 요청하는 콜러(Caller) 와 연결을 받는 콜리 (Calle)

