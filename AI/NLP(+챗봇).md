# nlp를 위한 데이터 수집 및 전처리

1. 웹 스크래핑하여 필요 텍스트만 추출하기 (bs4 or selenium 사용)
```python
import requests
import bs4

print ("You have successfully imported requests version "+requests.__version__)
print ("You have successfully imported beautifulsoup version "+bs4.__version__)

base_url = 'https://en.wikipedia.org/wiki/Jupiter'
# base_url = 'https://en.wikipedia.org/wiki/Jus21' # 404
r = requests.get(base_url)
print(r)

#`r.text`에는 이전에 GET 요청을 했을 때 반환된 원시 HTML이 포함되어 있습니다. 
#`'html5lib'`는 BeautifulSoup에 HTML 정보를 읽고 있다고 알려줍니다. 
soup = bs4.BeautifulSoup(r.text,'html5lib')
soup # == 페이지 소스 보기

headers = []
for url in soup.findAll("h3"):
    headers.append(url.text)
    print(url.text)
print(headers)

'''
i = len(headers) - 1
counter = 0
while counter <= i:
    if headers[counter].startswith('\n'):
        headers.pop(counter)
        counter -= 1
    counter += 1
    i = len(headers) -1
print(headers)
'''

####
r = requests.get(base_url)
soup = bs4.BeautifulSoup(r.text,'html5lib')
deet = soup.find('h3', text = headers[0]) # 클래스 'entry-content content' 의 div 태그 검색
para = deet.find_next_sibling('p') # 이 태그 안에서 모든 p 태그 검색
print(para.get_text())


r = requests.get(base_url)
soup = bs4.BeautifulSoup(r.text,'html5lib')
deet = soup.find('h3', text = headers[0]) # 클래스 'entry-content content' 의 div 태그 검색

for para in deet.find_next_siblings(): # 이 태그 안에서 모든 p 태그 검색
    if para.name == "h2" or para.name == "h3":
        break
    elif para.name == "p":
        print(para.get_text())

print(deet)
print(para)


# all_para 저장
r = requests.get(base_url)
all_para = ""
for header in headers:
    soup = bs4.BeautifulSoup(r.text,'html5lib')
    deet = soup.find('h3', text = header) # 클래스 'entry-content content' 의 div 태그 검색
    for para in deet.find_next_siblings(): # 이 태그 안에서 모든 p 태그 검색
        if para.name == "h2" or para.name == "h3":
            break
        elif para.name == "p":
            all_para += para.get_text()
            all_para += "\n"

print(all_para)

# 데이터 .txt 파일 저장
with open('./wiki.txt', 'wb') as file_handler:
        file_handler.write(all_para.encode('utf8'))

# %ls - 주피터에서만 쓸 수 있는지?
```


2. 네이버 영화 뉴스 top 10 웹 스크래핑
```python
from urllib.request import urlopen
from bs4 import BeautifulSoup

response = urlopen('https://entertain.naver.com/movie')
soup = BeautifulSoup(response, 'html.parser')

movie_news_titles = soup.find_all('a', {"class":"tit"})
movie_news_summaries = soup.find_all('p', {"class":"summary"})

movie_news = ""

for i, (title, summary) in enumerate(zip(movie_news_titles, movie_news_summaries)):
    movie_news += f"{title.text} \n {summary.text} \n" 
    movie_news += '\n'
    
with open("movie_news.txt", 'w', encoding = 'utf-8') as f:
  f.write("오늘의 영화뉴스 top 10\n")
  f.writelines(movie_news)
```


# 데이터 전처리
```python
import pandas as pd
import nltk

print ("You have successfully imported pandas version "+pd.__version__)
print ("You have successfully imported nltk version "+nltk.__version__)

# 자연 재해에 대한 트윗이 포함된 csv 파일을 pandas dataframe으로 읽습니다.
df_raw = pd.read_csv('[Dataset] Module 24 (socialmedia disaster tweets DFE).csv',encoding='ISO-8859-1')

print ("You have successfully loaded your csv file")

print(df_raw.info)# your code here
list(df_raw['text'].sample(5)) # 무작위

## 불필요한 단어 제거
## 데이터 집합에서 'stupid'라는 단어가 발생하는 횟수를 확인
frequecy_of_word=0
for i in range(len(df_raw)):
    x=df_text[i]
    if "stupid" in x:
        frequecy_of_word+=1
    else:
        continue

print("The frequency of the word stupid is :",frequecy_of_word)
```

# 토큰화
```python
sample_tweet = df_text.iloc[100]
print('Before tokenization:', sample_tweet)

nltk.download('punkt')
nltk.download('wordnet')
nltk.download('stopwords')

tokenized_tweet = nltk.tokenize.word_tokenize(sample_tweet)
print('After tokenization:', tokenized_tweet)
```

```python
tokenized_raw = [nltk.tokenize.word_tokenize(x) for x in list(df_text)] # 각 트윗이 토큰 목록이 되는 트윗 목록 만들기
len(set([y for x in tokenized_raw for y in x])) # 목록을 단일 목록으로 지정한 후 고유 토큰 수를 계산합니다.

# 어간 추출과 표제어 추출
import nltk
nltk.download()

porter = nltk.stem.PorterStemmer()
wnl = nltk.stem.WordNetLemmatizer()

stemmed = [porter.stem(word) for word in tokenized_tweet]
print(stemmed)
lemmed = [wnl.lemmatize(word) for word in tokenized_tweet]
print(lemmed)

# 금지어 제거
stop = nltk.corpus.stopwords.words('english')
# 여기에서 제거하고 싶은 금지어를 추가합니다.
stop.append('@')
stop.append('#')
stop.append('http')
stop.append(':')
stop
```

```python
# 데이터 프레임을 가져와서 토큰화된 트윗 반환환
def process_tweet(tweet):
    tokenized_tweet = nltk.tokenize.word_tokenize(tweet)
    stemmed = [porter.stem(word) for word in tokenized_tweet]
    processed = [w.lower() for w in stemmed if w not in stop]
    return processed

def tokenizer(df):
    tweets = []
    for _, tweet in df.iterrows():
        tweets.append(process_tweet(tweet['text']))
    return tweets

tweets = tokenizer(df_raw)

print(tweets)
print(len(set([y for x in tweets for y in x])))
```

```python
# 데이터 시각화
import matplotlib.pyplot as plt
def plot_hist(tweets):
    sentence_lengths = [len(tokens) for tokens in tweets]
    fig = plt.figure(figsize=(6, 6)) 
    plt.xlabel('Tweet length')
    plt.ylabel('Number of tweets')
    plt.hist(sentence_lengths, bins=20)
    plt.show()
    return sentence_lengths
tweet_lengths = plot_hist(tweets)
```

# 텍스트 데이터 분류
```python
import pandas as pd
import numpy as np
import nltk
import re

df_raw = pd.read_csv('[Dataset]_Module25_disasters_social_media.csv',encoding='ISO-8859-1')


```

# 정규식
다양한 정규식 사이트 사용법 안내 [https://inpa.tistory.com/entry/%F0%9F%92%BB-%EC%A0%95%EA%B7%9C%EC%8B%9D-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%82%AC%EC%9D%B4%ED%8A%B8-%F0%9F%8E%81-%EB%AA%A8%EC%9D%8C](https://inpa.tistory.com/entry/%F0%9F%92%BB-%EC%A0%95%EA%B7%9C%EC%8B%9D-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%82%AC%EC%9D%B4%ED%8A%B8-%F0%9F%8E%81-%EB%AA%A8%EC%9D%8C)  
    - 정규식 연습 사이트1 : [https://regexr.com/](https://regexr.com/) , [https://moonsupport.tistory.com/173](https://moonsupport.tistory.com/173)  
    - 정규식 연습 사이트2 : [https://regexper.com/](https://regexper.com/)



# 코사인 유사도
tf-idf  - 단어 빈도 * 역문서 빈도

### 유사도 이해1 
```python
# 유사도 이해하기 - 예시1
import numpy as np
from numpy import dot
from numpy.linalg import norm

'''
doc1 : 저는 사과 좋아요           
doc2 : 저는 바나나 좋아요
doc3 : 저는 바나나 좋아요 저는 바나나 좋아요

doc matrix : 바나나 사과 저는 좋아요 
- 길이가 비슷한 경우 유사도 높을 수 있지만 cosine 유사도를 사용하여 해결
'''

doc1 = np.array([0,1,1,1])
doc2 = np.array([1,0,1,1])
doc3 = np.array([2,0,2,2])

def cos_sim(A, B):
  return dot(A, B)/(norm(A)*norm(B))

print('doc1 과 doc2의 유사도 :',cos_sim(doc1, doc2).round(2))
print('doc1 과 doc3의 유사도 :',cos_sim(doc1, doc3).round(2))
print('doc2 와 doc3의 유사도 :',cos_sim(doc2, doc3).round(2))
```

### 유사도 이해2
```python
# 유사도 사용하기 - 예시2
import numpy as np

# 코사인 유사도 함수 정의
def cos_similarity(v1, v2):    
    dot_product = np.dot(v1, v2)        
    l2_norm = (np.sqrt(sum(np.square(v1))) * np.sqrt(sum(np.square(v2))))         
    similarity = dot_product / l2_norm                   
                              
    return similarity  

# Tfidf 구하기
from sklearn.feature_extraction.text import TfidfVectorizer

doc_list = ['if you take the blue pill, the story ends' ,
            'if you take the red pill, you stay in Wonderland',
            'if you take the red pill, I show you how deep the rabbit hole goes']

tfidf_vect_simple = TfidfVectorizer()
feature_vect_simple = tfidf_vect_simple.fit_transform(doc_list)

print(feature_vect_simple.shape)
print(type(feature_vect_simple))
print(feature_vect_simple)

# TFidfVectorizer로 transform()한 결과는 Sparse Matrix이므로 Dense Matrix로 변환. 
feature_vect_dense = feature_vect_simple.todense()

#첫번째, 두번째, 세번째 문장 feature vector 추출
vect1 = np.array(feature_vect_dense[0]).reshape(-1,)
vect2 = np.array(feature_vect_dense[1]).reshape(-1,)
vect3 = np.array(feature_vect_dense[2]).reshape(-1,)

#첫번째 문장과 두번째 문장의 feature vector로 두개 문장의 Cosine 유사도 추출
similarity_simple1 = cos_similarity(vect1, vect2)
similarity_simple2 = cos_similarity(vect1, vect3)
similarity_simple3 = cos_similarity(vect2, vect3)
print('doc1, doc2 코사인 유사도: {0:.3f}'.format(similarity_simple1))
print('doc1, doc3 코사인 유사도: {0:.3f}'.format(similarity_simple2))
print('doc2, doc3 코사인 유사도: {0:.3f}'.format(similarity_simple3))
```
cf. sparse matrix(희소 행렬 - 행렬 값 대부분 0)

### 유사도 이해3 - library 사용
```python
# 유사도 사용하기 - 예시3
from sklearn.metrics.pairwise import cosine_similarity

similarity_simple_pair = cosine_similarity(feature_vect_simple , feature_vect_simple)
print(similarity_simple_pair)
```


# 유사도(cosine_similarity)를 이용한 영화 추천 시스템 
```python
import pandas as pd
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity

data = pd.read_csv('movies_metadata.csv', low_memory=False)
df = data.head(20000)
#df['overview'].isnull().sum()
df.loc[:, 'overview'] = df['overview'].fillna('')

# tfidf 구하기
tfidf = TfidfVectorizer(stop_words='english')
tfidf_matrix = tfidf.fit_transform(df['overview'])

cosine_sim = cosine_similarity(tfidf_matrix, tfidf_matrix)

title_to_index = dict(zip(df['title'], df.index))

def get_recommendations(title, cosine_sim=cosine_sim):
    # 선택한 영화의 타이틀로부터 해당 영화의 인덱스를 받아온다.
    idx = title_to_index[title]

    # 해당 영화와 모든 영화와의 유사도를 가져온다.
    sim_scores = list(enumerate(cosine_sim[idx]))

    # 유사도에 따라 영화들을 정렬한다.
    sim_scores = sorted(sim_scores, key=lambda x: x[1], reverse=True)

    # 가장 유사한 10개의 영화를 받아온다. 
    # 0번 인덱스에는 선택한 영화와의 유사도가 저장되어 있어 이를 제외하고 나머지 영화들(1번부터 10번까지)의 유사도를 가져옴
    sim_scores = sim_scores[1:11]

    # 가장 유사한 10개의 영화의 인덱스를 얻는다.
    movie_indices = [idx[0] for idx in sim_scores]

    # 가장 유사한 10개의 영화의 제목을 리턴한다.
    return df['title'].iloc[movie_indices]

get_recommendations('The Dark Knight Rises')
```


**표제어 추출 (Lemmatization)**:
- 표제어 추출은 단어의 원형을 찾아냅니다. 단어의 원형은 사전에 등재된 형태로, 단어의 의미를 가장 정확하게 나타냅니다.
- 예를 들어, "running"은 "run"의 표제어이며, "better"는 "good"의 표제어입니다.
**어간 추출 (Stemming)**:
- 어간 추출은 단어에서 접사를 제거하여 어간만을 남깁니다. 어간은 단어의 핵심 의미를 나타냅니다.
- 예를 들어, "running"의 어간은 "run"이며, "better"의 어간은 "better"입니다.

# 챗봇 만들기
```python
1. 라이브러리 가져오기
2. 데이터 가져오기
3. 데이터 전처리하기
   1) 소문자로 변환
   2) 단어 토큰화하기 - word_tokenize()사용 or sent_tokenize()
   3) 표제어 추출함수 정의하기 - nltk.stem.WordNetLemmatizer() 사용
   4) 정규화 함수 정의하기 - 구두점 제거하고 표제어 추출하기
4. 응답함수 정의하기
   1) 질문 문장을 기존 토큰에 추가하기
   2) 토큰에 대해 tfidf 생성하기
   3) 질문 문장과 기존 토큰과의 코사인 유사도 구하기
   4) 코사인 유사도 값이 가장 큰 값 구하기
   5) 코사인 유사도 값이 가장 큰 문장 + 유사도값 출력하기
5. 인삿말 대응 함수 정의하기 - greeting()
6. 끝인삿말 대응 함수 정의하기 - goodbye()
7. 챗봇 사용하기
    1) 안내문 띄우기
    2) 질문 입력받기
    3) 질문 소문자 변경하기
    4) 인삿말 - 인사말대응, 끝인삿말-끝인삿말대응, 질문-챗봇응대
```

```python
# 라이브러리 가져오기
import nltk # 텍스트 데이터를 처리하기 위한 것
import numpy as np # 말뭉치를 배열로 나타내기 위한 것
import random 
import string # 표준 파이썬 문자열을 처리
from sklearn.metrics.pairwise import cosine_similarity # 이를 나중에 사용하여 두 개의 문장이 얼마나 비슷한지를 결정합니다.
from sklearn.feature_extraction.text import TfidfVectorizer # Experience 2에서 단어 가방을 만드는 함수를 만들었던 것을 기억하십니까? 이 함수는 같은 일을 합니다!

# nltk 모듈 - nltk 제공하는 리소스
nltk.download('punkt')
nltk.download('wordnet')

# 데이터 가져오기
filepath = './robots.txt'
corpus = open(filepath, 'r', errors = 'ignore')
raw_data = corpus.read()
print(raw_data)

# 데이터 전처리 하기
# 1) 소문자 변환
raw_data = raw_data.lower()

# 토큰화 - 문장
sent_tokens = nltk.sent_tokenize(raw_data)
print(*sent_tokens, sep = '\n') # * 언패킹 연산자 

# 토큰화 - 단어
word_tokens = nltk.word_tokenize(raw_data)
print(word_tokens)

# 표준화 추출함수 정의 - nltk.stem.WordNetLemmaatizer() 사용
lemmer = nltk.stem.WordNetLemmatizer() # lemmer 클래스를 초기화합니다. WordNet은 NLTK에 포함된 의미 론적 영어 사전입니다
def LemTokens(tokens):
    return [lemmer.lemmatize(token) for token in tokens]

# 정규화 함수 정의 - 구두점 제거하고 표제어 추출 
remove_punct_dict = dict((ord(punct), None) for punct in string.punctuation)
def LemNormalize(text):
    return LemTokens(nltk.word_tokenize(text.lower().translate(remove_punct_dict))) #see previous section 1.2.5 lemmatization

test_sentence = 'Today was a wonderful day. The sun was shining so brightly and the birds were chirping loudly!'

test_word_tokens = nltk.word_tokenize(test_sentence)  # 문서를 단어 목록으로 변환 list 타입

lemmer = nltk.stem.WordNetLemmatizer()
def LemTokens(tokens):
    return [lemmer.lemmatize(token) for token in tokens]

print(LemTokens(test_word_tokens)) # list 매개변수
print(LemNormalize(test_sentence)) # str 매개변수

# 응답함수 정의
def response(user_response):
    
    robo_response='' # 문자열을 포함하도록 변수를 초기화
    sent_tokens.append(user_response) # 질문 문장을 기존 토큰에 추가
    TfidfVec = TfidfVectorizer(tokenizer=LemNormalize, stop_words='english') 
    tfidf = TfidfVec.fit_transform(sent_tokens) # tfidf 생성
    vals = cosine_similarity(tfidf[-1], tfidf) # 질문 문장과 기존 토큰과의 코사인 유사도 구하기
    idx=vals.argsort()[0][-2] # 코사인 유사도 값이 가장 큰 값 구하기
    flat = vals.flatten() 
    flat.sort() # 오름차순으로 정렬
    req_tfidf = flat[-2] 
    
    if(req_tfidf==0):
        robo_response=robo_response+"I am sorry! I don't understand you"
        return robo_response
    else:
        robo_response = robo_response+sent_tokens[idx] # 코사인 유사도 값이 가장 큰 문장 + 유사도 값 출력
        return robo_response

GREETING_INPUTS = ["hello", "hi", "greetings", "sup", "what's up","hey", "hey there"]
GREETING_RESPONSES = ["hi", "hey", "*nods*", "hi there", "hello", "I am glad! You are talking to me"]

# 인삿말 대응 함수 정의 - greeting()
def greeting(sentence):
    for word in sentence.split():
        if word.lower() in GREETING_INPUTS:
            return random.choice(GREETING_RESPONSES)


GOODBYE_INPUTS = ["goodbye", "farewell", "see you later", "bye for now", "take care", "i'm out"]
GOODBYE_RESPONSES = ["goodbye take care", "see you", "farewell! have a good one!", "good bye! catch you later!", "take care", "alright, see you next time"]

# 끝인삿말 대응 함수 정의 - goodbye()
def goodbye(sentence):
    for word in sentence.split():
        if word.lower() in GOODBYE_INPUTS:
            return random.choice(GOODBYE_RESPONSES)
```

### 챗봇 사용하기
```python
flag=True
# 안내문 띄우기
print("ROBO: My name is Robo. I will answer your queries about Chatbots. If you want to exit, press q!")
while(flag==True):
    user_response = input() # 질문 입력받기
    user_response=user_response.lower() # 질문 소문자 변경
    if(user_response!='q'):
        if(goodbye(user_response) != None):
            flag=False
            print("ROBO: " + goodbye(user_response))
        else:
            if(greeting(user_response) != None):
                print("ROBO: "+greeting(user_response))
                # tell_time 함수를 작성했으면 아래의 문장을 주석해제하세요.
#             if(tell_time(user_response)!=None):
#                 print("ROBO: "+tell_time(user_response))
            else:
                print("ROBO: ",end="")
                print(response(user_response))
                sent_tokens.remove(user_response)
    else:
        flag=False
        print("ROBO: Bye! turn off................")
```