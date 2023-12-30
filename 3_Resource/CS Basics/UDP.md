User Datagram Protocol

`connectionless`: 연결을 맺지 않고 바로 데이터를 주고 받는다.
(TCP는 [[connection-oriented]])

`unreliable`: IP (Internet Protocol)을 거의 그대로 사용.

초기엔 UDP에 [[socket]]개념이 없다가 새로 등장하면서 [[socket]]을 식별하는데 protocol(tcp, udp)도 추가되었다.

socket = protocol + ip address + port