[openai Api kotlin github](https://github.com/aallam/openai-kotlin/blob/main/README.md)

window 환경변수 - android studio는 아니고 사용 시
[공식문서](https://help.openai.com/en/articles/5112595-best-practices-for-api-key-safety)
문서 및 박사님 말로는 백엔드에서 api key를 받아 쓰기.

local.properties 활용
[api key 관리](https://kkong-93.tistory.com/64)
[manifest에서 사용하는 것 까지](https://jgeun97.tistory.com/296)

![[Pasted image 20231211192952.png]]
생성되었음.

[openAI API assistant python 블로그](https://tilnote.io/pages/6553094c5c322232e01b8b31)

### Assistant

Assistant는 모델과 도구를 사용하여 작업을 수행할 수 있는 도구입니다. Assistant를 생성하면 모델과 도구를 설정할 수 있습니다. Assistant는 여러 도구를 포함할 수 있으며, 생성, 수정, 삭제 등의 작업이 가능합니다. Assistant는 작업을 수행할 때 대화 스레드와 연결될 수 있습니다.

### Thread

Thread는 Assistant와 사용자 간의 대화를 나타냅니다. 사용자가 Assistant에게 메시지를 보내면 이 메시지는 특정 Thread에 속하게 됩니다. Assistant는 Thread에 연결되어 해당 Thread의 메시지에 답할 수 있습니다. 각 Thread는 ID를 가지고 있어 구분이 가능합니다.

### Run

Run은 Assistant가 특정 Thread에서 특정 작업을 수행하는 인스턴스입니다. Assistant는 특정 Thread에서 Run을 생성하여 작업을 수행할 수 있습니다. 각 Run은 Assistant가 수행한 특정 작업을 나타내며, 상태와 메타데이터를 포함합니다. Run은 생성, 수정, 취소 등의 작업이 가능합니다.

### Messages

Messages는 Assistant와 사용자 간의 대화의 구성 요소입니다. 각 메시지는 특정 Role(User 또는 System)을 가지고 있으며, 내용을 포함합니다. Assistant와 사용자 간의 상호 작용은 메시지를 통해 이루어지며, Assistant는 메시지에 따라 응답합니다. Messages는 Thread에 속하게 되며, 여러 메시지로 대화가 구성됩니다.

### 대화창 구분

앱에서는 각 Thread에 대한 대화창을 구성하여 사용자와 Assistant 간의 대화를 나눌 수 있습니다. 각 Thread에는 해당하는 대화창이 있고, 각 메시지의 Role을 통해 User와 Assistant의 메시지를 구분할 수 있습니다. 사용자와 Assistant 간의 대화를 시각적으로 나누기 위해 UI에서는 Role에 따라 다른 스타일이나 색상을 사용할 수 있습니다.

대화창을 표시할 때는 Thread와 해당하는 Messages를 사용하여 구성하고, User와 Assistant의 메시지를 구분하여 표시하면 됩니다. 예를 들어, User 메시지는 한 색상, Assistant 메시지는 다른 색상 등을 사용하여 시각적으로 구분할 수 있습니다.
