
실습에서는 vscode에 requirements.txt, main.py, .env를 만들라고 나와있는데
pycharm에서 .env 파일이 시스템 파일로 인식을 못하는 것 같아서 plugin을 설치하려다
[stackoverflow](https://stackoverflow.com/questions/42708389/how-to-set-environment-variables-in-pycharm) 에서 pycharm env 변수 설정 방법을 통해 해결.


```python
# install
pip install python-dotenv 
# .env로 환경 변수 관리 pycharm에서 vsc처럼 시스템 이모지로 변하진 않았음
pip install langchain
pip install openai # api 키는 연습용으로 강사님 꺼 썼음
pip install streamlit # 빠르게 front 구성해주는 streamlit
pip install pypdf # chatpdf 만들기 위함
pip install tiktoken # embedding model 사용
pip install chromadb # 
```

