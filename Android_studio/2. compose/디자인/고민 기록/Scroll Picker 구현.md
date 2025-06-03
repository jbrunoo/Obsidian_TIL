
```kotlin
@Composable
private fun ScrollPicker(
    selectedOption: TitleOption,
    options: List<TitleOption>,
    modifier: Modifier = Modifier,
    visibleOptionCount: Int = 5,
    onChangedOption: (Int) -> Unit,
) {
	// 선택된 옵션을 중앙에 위치하기 위해 상하로 빈 option List 추가
    val median = visibleOptionCount / 2
    val spaceOptions = List(median) { null }
    val adjustedOptions = spaceOptions + options + spaceOptions
    val listCount = adjustedOptions.size

	// 실제 options가 들어있는 index에서만 text 가져오기
    @Composable
    fun getTitleText(index: Int): String =
        adjustedOptions.getOrNull(index)?.let { (round, subject) ->
            "$round" + stringResource(R.string.scroll_picker_title_infix_text) + subject
        } ?: AppConst.EMPTY_STRING

    val listState =
        rememberLazyListState(initialFirstVisibleItemIndex = selectedOption.round - 1)
    val flingBehavior = rememberSnapFlingBehavior(lazyListState = listState)

    val currentCenterIndex = remember {
        derivedStateOf {
            listState.firstVisibleItemIndex + median
        }
    }

    val fadingEdgeGradient = Brush.verticalGradient(
        0f to Color.Transparent,
        0.5f to Color.Black,
        1f to Color.Transparent
    )

    val boxHeight = 264.dp
    val itemSize = 28.dp

    val upperDividerYOffset = boxHeight / 2 - itemSize / 2
    val underDividerYOffset = boxHeight / 2 + itemSize / 2

    LaunchedEffect(listState) {
        snapshotFlow { listState.firstVisibleItemIndex }
            .filter { it in options.indices }
            .distinctUntilChanged()
            .collect { round -> onChangedOption(round) }
    }

    Box(
        modifier = modifier
            .fillMaxWidth()
            .height(height = boxHeight)
            .innerShadow(
                shape = RectangleShape,
                blur = 0.dp,
                offsetX = 0.dp,
                offsetY = (-0.5).dp,
            )
            // 해당 피커가 바텀 시트 내에 구현되어 있는 상황.
            // 해당 로직을 통해 스크롤 피커 내 LazyColumn에서 상/하단 end까지 스크롤이 완료되면 
            // 남아있는 스크롤 이벤트를 바로 상위 composable에서 다 소비하여 바텀 시트 내에서 
            // 이벤트가 넘어가지 않다록 소비시키는 역할.
            .nestedScroll(object : NestedScrollConnection {
                override fun onPostScroll(
                    consumed: Offset,
                    available: Offset,
                    source: NestedScrollSource,
                ): Offset = available
            }),
    ) {
        HorizontalDivider(
            modifier = Modifier.offset(y = upperDividerYOffset),
            thickness = 0.5.dp,
            color = Color(0xFFCCCCCC)
        )

        HorizontalDivider(
            modifier = Modifier.offset(y = underDividerYOffset),
            thickness = 0.5.dp,
            color = Color(0xFFCCCCCC)
        )

        LazyColumn(
            state = listState,
            flingBehavior = flingBehavior,
            horizontalAlignment = Alignment.CenterHorizontally,
            modifier = Modifier
                .align(Alignment.Center)
                .wrapContentSize()
                .height(itemSize * visibleOptionCount)
                .fadingEdge(fadingEdgeGradient)
        ) {
            items(
                count = listCount,
                key = { it },
            ) { index ->
                val distanceFromCenter = kotlin.math.abs(index - currentCenterIndex.value)
                val fontSize = when (distanceFromCenter) {
                    0 -> 23.sp
                    1 -> 18.sp
                    2 -> 14.sp
                    else -> 12.sp
                }

                Box(
                    modifier = Modifier.height(itemSize),
                    contentAlignment = Alignment.Center,
                ) {
                    SGText(
                        text = getTitleText(index = index),
                        style = getSGNonScaleTextStyle(
                            color = SGColor.Grayscale.black100,
                            fontSize = fontSize,
                            fontWeight = FontWeight.Normal,
                            lineHeight = 20.sp,
                            fontFamily = SGTypography.pretendard,
                        )
                    )
                }
            }
        }
    }
}
```