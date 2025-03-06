#### basic.
	ux 관점 - 무한스크롤 vs pagination

	pagination 기법 (offset, cursor)
		offset: page기반 (sql 상 limit, offset)
		특정 페이지 접근, 전체 페이지 조회 수월 <-> 뒤쪽 페이지 성능 저하, 데이터 추가/삭제 시 불일치 가능성
		
		cursor: 기준점 기반, 정확히 마지막 데이터 이후로 가져옴
		데이터 일관성, 대규모 데이터 성능 좋음 <-> 연속적이지 않은 페이지 이동 어려움, 구현 난이도 증가

- - -
현재 디자인 상 무한 스크롤 + offset 기반

paging3 라이브러리를 놔두고 사용하지 않은 이유.

1. 로컬 db를 사용하지 않고 api만으로 호출하고 있었다.
	remoteMediator 기능을 사용하지 않았고 무거운 라이브러리를 굳이 추가할 필요없다고 판단하였다.
2. MVI 패턴을 활용중인데 State의 single source of truth를 지키기 위해 불변해야하고 단일 State로 보통 관리를 한다. State라고 하기에는 paging3 공식문서에서 flow로 가져오는 것을 권장하고 이는 스트림 형태라 viewmodel 내에서 단일 State로 관리가 안되거나 작업을 돌아돌아 하는 부분이 있다고 판단하였다.

백엔드에서 page, pageSize, hasNext와 같은 pageInfo를 내려주어서 아래와 같은 State를 최종 만들었다.

```kotlin
internal data class HomeGalleryUIState(  
    val isLoading: Boolean = false,  
    val isShowBottomSheet: Boolean = false,  
    val isRefreshing: Boolean = false,  
    val postOrderType: PostOrderType = PostOrderType.MOST_LIKED,  
    val paginationStatus: PaginationStatus = PaginationStatus.LOADING,  
    val posts: List<GalleryPostDto> = emptyList(),  
    val nextPage: Int = 0,  
    val hasNextPage: Boolean = false,  
) : UIState {  
  
    override fun toLoggingElements(): Array<LogElementArgument> = arrayOf(  
        LogElementArgument("isLoading", isLoading.toString()),  
        LogElementArgument("isShowBottomSheet", isShowBottomSheet.toString()),  
        LogElementArgument("postOrderType", postOrderType.toString()),  
        LogElementArgument("paginationStatus", paginationStatus.toString()),  
        LogElementArgument("posts", posts.toString()),  
    )  
}

// 페이징 상태
enum class PaginationStatus {  
    LOADING, // loading first page  
    INACTIVE, // no loading  
    PAGINATING, // loading next page  
    EXHAUST, // loaded all pages  
    ERROR, // fail loaded  
    EMPTY, // success loaded - no content  
}
```

nextPage나 hasNextPage는 PaginationStatus가 있기 때문에 viewmodel에서 private으로 관리되어도 되는 값인데 MVI 패턴에 좀 더 맞는 방향인가 싶어서 작성하였다.

```kotlin
/* HomeGalleryViewModel 내 */

// fetchPostPage가 실행되면 loading인지 paginating인지 구분
private fun setUpLoading(isInitPage: Boolean, isRefreshing: Boolean) {  
    if (isInitPage) {  
        reduce {  
            copy(  
                isRefreshing = isRefreshing,  
                paginationStatus = PaginationStatus.LOADING,  
                posts = emptyList()  
            )  
        }  
    } else {  
        reduce {  
            copy(  
                paginationStatus = PaginationStatus.PAGINATING,  
            )  
        }  
    }  
}  

private suspend fun fetchPostPage(page: Int = 0, isRefreshing: Boolean = false) = launch {  
	// 불필요한 api 호출 방지
    if (currentState.paginationStatus in listOf(  
            PaginationStatus.LOADING,  
            PaginationStatus.PAGINATING,  
            PaginationStatus.EXHAUST  
        )  
    ) return@launch  
  
    val currentPage = if(isRefreshing) 0 else page  
  
    setUpLoading(isInitPage = currentPage == 0, isRefreshing = isRefreshing)  
  
    getGalleryUseCase(  
        params = GetGalleryUseCase.Params(  
            round = null,  
            orderType = currentState.postOrderType.name,  
            page = currentPage,  
            pageSize = PAGE_SIZE,  
        )  
    ).onSuccess { galleryDto ->  
        reduce {  
            copy(  
                isRefreshing = false,  
                paginationStatus = when {  
                    !galleryDto.hasNext -> PaginationStatus.EXHAUST  
                    galleryDto.posts.isEmpty() -> PaginationStatus.EMPTY  
                    else -> PaginationStatus.INACTIVE  
                },  
                posts = posts + galleryDto.posts,  
                nextPage = currentPage + 1,  
                hasNextPage = galleryDto.hasNext  
            )  
        }  
    }.onFailure {  
        reduce {  
            copy(  
                isRefreshing = false,  
                paginationStatus = PaginationStatus.ERROR  
  
            )  
        }  
    }
}
```

새로고침의 경우와 페이지의 첫 로딩인지 이후 로딩인지를 한 함수 내에서 구분을 지었다.
fetch 관련 로직이 suspend인데 reduce를 계속해주어야 하기 때문에 
코드가 늘어나는 것보다 코드를 통합시켰다.
불필요한 코드들이 있긴 하지만 isRefreshing이 변하지 않았는데 값을 계속 copy 하는 등.
비용이 크지 않다고 판단하였다. (다른 방향으로 리펙토링하는 방법도 찾아보는게 좋을 것 같다.)

UI 에서는 paging이 일어날 때 각 intent를 연결해주면 되는데,
refreshing은 Material3 용 `rememberPullToRefreshState()`, `PullToRefreshBox` 가 존재한다.
nextloading을 위한 스크롤 범위를 측정하고 launchedEffect를 적절히 발생시키는 작업이 현재 남아있다.

위 작업이 끝나면 이미지 캐싱 등의 작업을 처리해볼 예정이다.