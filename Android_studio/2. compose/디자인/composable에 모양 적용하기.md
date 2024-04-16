[medium](https://juliensalvi.medium.com/custom-shape-with-jetpack-compose-1cb48a991d42)

modifier 기능 활용
- **_background(color, shape)_** : 지정된 색상 또는 브러시를 사용하여 컴포저블의 새 윤곽선을 정의.
- **_graphicLayer{}_** : **_모양_** 속성을 설정하여 새 윤곽선을 정의.그림자 고도가 윤곽선과 일치하도록 하려면 **_클립을_** _true_ 로설정
- **_border(width, color, shape)_** : 지정된 모양, 너비, 색상으로 컴포저블 주위에 테두리를 그리는 데 사용.
- **_drawBehind{}_** : DrawScope 및 Canvas에 액세스하여 컴포저블의 윤곽선을 그림.

