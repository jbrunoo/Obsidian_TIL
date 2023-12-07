
파이썬 
request.post 사용하면 API에 요청 보내서 쓸 수 있음
but, 최신 버전에서는 쓸 수 없음

최신 버전은 이전 대화를 기억하는 기능
즉, 안스에서 개발하면 이전 대화 기억 기능은 사용할 수 없음

1번 qna - API 사용, pip install openai
```python
from openai import OpenAI  
  
client = OpenAI(api_key = '[api-key]')  
  
  
messages = [ {"role": "system", "content": "You are a intelligent assistant."} ]  
  
while True:  
    message = input("User : ")  
    if message:  
        messages.append(  
            {"role": "user", "content": message},  
        )  
        chat = client.chat.completions.create(  
            model="gpt-3.5-turbo", messages=messages  
        )  
    else:  
      print("Chatting is Ended!")  
      break  
  
    reply = chat.choices[0].message.content  
    print(f"ChatGPT: {reply}")  
    messages.append({"role": "assistant", "content": reply})
```

2번 qna - http post 방식, pip install requests
```python
import requests  
import openai  
  
# Set up the API endpoint and access token  
api_endpoint = "https://api.openai.com/v1/chat/completions"  
openai.api_key = '[api-key]'  
  
# Define the input question  
input_question = input('질문을 입력하세요 : ')  
  
# Define the payload with the input question and access token  
payload = {  
    "model" : "gpt-3.5-turbo",  
    "messages" : [ {"role": "user", "content": input_question} ],  
    "temperature": 1.0,  
    "top_p": 1.0,  
    "n": 1,  
    "stream": False,  
    "presence_penalty": 0,  
    "frequency_penalty" : 0  
}  
  
headers = {  
    "Content-Type" : "application/json",  
    "Authorization" : f"Bearer {openai.api_key}"  
}  
  
# Send a POST request to the NLP API for question answering  
response = requests.post(api_endpoint, headers=headers, json=payload, stream = False)  
  
#print(response.json())  
# Parse the response to extract the generated answer  
if response.status_code == 200:  
    result = response.json()  
    generated_answer = result["choices"][0]["message"]["content"]  
    print("Generated Answer:", generated_answer)  
else:  
    print("Error:", response.status_code)  
  
#print(result)
```


prompt engineering
1. 구체적
2. simple, 명확
3.  절차적으로
4. 예시를 들어서
5. zero-show prompts : 
	명시적인 학습이나 지도 데이터 없이도 작업을 수행하는데 사용되는 텍스트 입력의 형태
6. few shot prompts : 
	몇 가지 예시를 통해, 작업을 학습하고, 그러한 예시를 통해 작업을 수행하는 방법
7. 조건문 달기
8. 최대 몇단어까지 사용
9. B에 집중해서 작성
10. HTML 포맷으로 만들어줘
11. gptable.net (대구 지방 회사가 만든 사이트)


새로운 기능 [assistants]([https://platform.openai.com/assistants](https://platform.openai.com/assistants)) 
promgram에서 처리하려면 이전 메시지를 append 하면서 저장하는 방식.

이것은 대화 Thread를 만들고 assistants 연결하고 
(http API 방식으론 지원하지 않고 API 방식만 지원 중)
```python
client = OpenAI(api_key = '[api-key]')  
  
  
#https://platform.openai.com/assistants 이 사이트에서 Assistant 관리  
tour_assistant_id = '[assistants-id]'  
TOUR_ASSISTANT_ID = tour_assistant_id  # or a hard-coded ID like "asst-..."
# thread 만들고 assistants 연결
def create_thread_and_run(user_input):  
    thread = client.beta.threads.create()  
    run = submit_message(TOUR_ASSISTANT_ID, thread, user_input)  
    return thread, run
# thread에 이전 대화 저장
def submit_message(assistant_id, thread, user_message):  
    client.beta.threads.messages.create(  
        thread_id=thread.id, role="user", content=user_message  
    )  
    return client.beta.threads.runs.create(  
        thread_id=thread.id,  
        assistant_id=assistant_id,  
    )
def get_response(thread):  
    return client.beta.threads.messages.list(thread_id=thread.id, order="asc")  
# Pretty printing helper  
def pretty_print(messages):  
    print("# Messages")  
    for m in messages:  
        print(f"{m.role}: {m.content[0].text.value}")  
    print()  
#답을 가져올 때까지 기다리는 함수  
def wait_on_run(run, thread):  
    while run.status == "queued" or run.status == "in_progress":  
        run = client.beta.threads.runs.retrieve(  
            thread_id=thread.id,  
            run_id=run.id,  
        )  
        time.sleep(0.5)  
    return run  
  
thread1, run1 = create_thread_and_run(  
    "부산에서 관광할 만한 다섯곳 추천해주세요"  
)  
run1 = wait_on_run(run1, thread1)  
pretty_print(get_response(thread1))  
  
run2 = submit_message(TOUR_ASSISTANT_ID, thread1, "첫번째 장소를 추천한 이유는?")  
run2 = wait_on_run(run2, thread1)  
pretty_print(get_response(thread1))
```

