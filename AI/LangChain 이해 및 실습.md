
- python
- streamlit - 파이썬 활용 프론트
- langchain - LLM 어플리케이션 개발 프레임워크(LLM과 DB 연결)
- chroma - DB, pdf 저장
- LLaMA 2 - 오프라인 chatgpt (meta)



LLaMA 2 경량화버전 : Georgi Gerganov Machine Learning
[Hugging Face](https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGML/blob/main/llama-2-7b-chat.ggmlv3.q2_K.bin) Files and Versions 에서 가장 용량 작은 모델 다운 받음.

```python
import streamlit as st  
st.title('시를 짓는 인공지능')  
  
#from langchain.chat_models import ChatOpenAI  
# Chat_model = ChatOpenAI()  
  
from langchain.llms import CTransformers  
  
llm = CTransformers(  
    model ='llama-2-7b-chat.ggmlv3.q2_K.bin',  
    model_type = 'llama'  
)  
  
# text_input() 샤용해보기  
content = st.text_input('시의 주제를 제시해 주세요')  
  
if st.button('시 작성 요청하기'):  
    with st.spinner('시를 작성 중입니다.'):  
        result = llm.predict( 'write o poem about' + content + ':')  
        st.write(result)
```

```python
# 터미널에서
streamlit run main.py # 실행, 웹 사이트에서 사용 가능
```

