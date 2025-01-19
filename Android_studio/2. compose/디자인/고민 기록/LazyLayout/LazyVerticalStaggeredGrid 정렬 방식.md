LazyLayout을 사용하는 Composable인데 높이가 다른 item을 배치할 수 있는 컴포넌트이다.

생각없이 사용하다가, item이 홀수일 때, 오른쪽으로 아이템이 정렬된 모습을 발견했다.

![[Pasted image 20250118181331.png|200]]

사실 오른쪽으로 정렬된 것은 이상하지 않다. 왜냐하면 staggeredGrid는 높이가 다른 item들을 배치하기 때문에
measure할 때 아이템의 높이가 더 낮은 쪽부터 아이템을 채워넣기 때문에 마지막에 오른쪽이 튀어나오는 경우가 생길 수 있다.

하지만 위 프리뷰는 같은 height를 가지는 아이템에도 불구하고 오른쪽으로 아이템이 추가된 것을 확인할 수 있다.

```kotlin
// LazyLayout은 measurePolicy를 받는다.
@ExperimentalFoundationApi
@Composable
public fun LazyLayout(
    itemProvider: () -> LazyLayoutItemProvider,
    modifier: Modifier = Modifier,
    prefetchState: LazyLayoutPrefetchState? = null,
    measurePolicy: LazyLayoutMeasureScope.(Constraints) -> MeasureResult
): Unit

// staggeredGrid의 MeasurePolicy는 다음과 같지만, 살펴보지는 않겠다.
// 어짜피 measure 부분을 건드릴 수 없고 학습하여 처음부터 구현해서 사용할 것도 아니기 때문이다.
@OptIn(ExperimentalFoundationApi::class)  
@Composable  
internal fun rememberStaggeredGridMeasurePolicy(  
    state: LazyStaggeredGridState,  
    itemProviderLambda: () -> LazyStaggeredGridItemProvider,  
    contentPadding: PaddingValues,  
    reverseLayout: Boolean,  
    orientation: Orientation,  
    mainAxisSpacing: Dp,  
    crossAxisSpacing: Dp,  
    coroutineScope: CoroutineScope,  
    slots: LazyGridStaggeredGridSlotsProvider,  
    graphicsContext: GraphicsContext  
): LazyLayoutMeasureScope.(Constraints) -> LazyStaggeredGridMeasureResult = remember(  
    state,  
    itemProviderLambda,  
    contentPadding,  
    reverseLayout,  
    orientation,  
    mainAxisSpacing,  
    crossAxisSpacing,  
    slots,  
    graphicsContext  
) {  
    { constraints ->  
        state.measurementScopeInvalidator.attachToScope()  
        checkScrollableContainerConstraints(  
            constraints,  
            orientation  
        )  
        val resolvedSlots = slots.invoke(density = this, constraints = constraints)  
        val isVertical = orientation == Orientation.Vertical  
        val itemProvider = itemProviderLambda()  
  
        // setup measure  
        val beforeContentPadding = contentPadding.beforePadding(  
            orientation, reverseLayout, layoutDirection  
        ).roundToPx()  
        val afterContentPadding = contentPadding.afterPadding(  
            orientation, reverseLayout, layoutDirection  
        ).roundToPx()  
        val startContentPadding = contentPadding.startPadding(  
            orientation, layoutDirection  
        ).roundToPx()  
  
        val maxMainAxisSize = if (isVertical) constraints.maxHeight else constraints.maxWidth  
        val mainAxisAvailableSize = maxMainAxisSize - beforeContentPadding - afterContentPadding  
        val contentOffset = if (isVertical) {  
            IntOffset(startContentPadding, beforeContentPadding)  
        } else {  
            IntOffset(beforeContentPadding, startContentPadding)  
        }  
  
        val horizontalPadding = contentPadding.run {  
            calculateStartPadding(layoutDirection) + calculateEndPadding(layoutDirection)  
        }.roundToPx()  
        val verticalPadding = contentPadding.run {  
            calculateTopPadding() + calculateBottomPadding()  
        }.roundToPx()  
  
        val pinnedItems = itemProvider.calculateLazyLayoutPinnedIndices(  
            state.pinnedItems,  
            state.beyondBoundsInfo  
        )  
  
        // todo: wrap with snapshot when b/341782245 is resolved  
        val measureResult =  
            measureStaggeredGrid(  
                state = state,  
                pinnedItems = pinnedItems,  
                itemProvider = itemProvider,  
                resolvedSlots = resolvedSlots,  
                constraints = constraints.copy(  
                    minWidth = constraints.constrainWidth(horizontalPadding),  
                    minHeight = constraints.constrainHeight(verticalPadding)  
                ),  
                mainAxisSpacing = mainAxisSpacing.roundToPx(),  
                contentOffset = contentOffset,  
                mainAxisAvailableSize = mainAxisAvailableSize,  
                isVertical = isVertical,  
                reverseLayout = reverseLayout,  
                beforeContentPadding = beforeContentPadding,  
                afterContentPadding = afterContentPadding,  
                coroutineScope = coroutineScope,  
                graphicsContext = graphicsContext  
            )  
        state.applyMeasureResult(measureResult)  
        measureResult  
    }  
}
```

[[lazyColumn의 크기는 가져오지 못할까]] 에서 잠깐 언급했듯,
subCompose라는 키워드가 있다.

[subCompose 정리된 블로그](https://everytimegreen.tistory.com/35)
위 블로그 내용처럼 compose의 동작방식 중,
composition -> layout(measure, placement) -> Drawing 과정을 거치는데,
layout의 사이에 subcompose가 동작하게 된다.
ex) measure - "subcomposition" - palcement


현재 위 방법으로 마지막 아이템 2개의 측정된 높이를 가져와서 같을 경우, 왼쪽으로 정렬되게 하는 방법을 이론적으로 생각해보고 있다. 현재 작업 pr하게 되면 남은 시간에 도전해볼 생각이다.