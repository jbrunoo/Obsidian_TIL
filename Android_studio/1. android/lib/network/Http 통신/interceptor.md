[medium](https://medium.com/@myofficework000/retrofit-interceptors-for-beginners-76943e987ad5)

application interceptor(addInterceptor) : applicaion <-> OKHttp core lib 
network interceptor(addNetworkInterceptor) : OKHttp <-> server

1. HttpLoggingInterceptor
	BASIC, BODY, HEADERS, NONE
2. cache interceptor
	 응답을 캐시
3. auth interceptor
	header 추가하여 token, jwt 등 전달


api interceptor에서 addQueryParameter -> addPathSegment 이용하여 api, json 등 path 추가했는데
baseUrl  + pathSegment + endpoint 원했는데 baseUrl + endpoint + pathSegment 순으로 나옴.
