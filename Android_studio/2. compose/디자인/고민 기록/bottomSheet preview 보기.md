```kotlin
@OptIn(ExperimentalMaterial3Api::class)  
@Composable  
internal fun HomeScreenBottomSheet(  
    modifier: Modifier = Modifier,  
    sheetState: SheetState = rememberModalBottomSheetState(skipPartiallyExpanded = false),  
    closeSheet: () -> Unit  
) {  
    ModalBottomSheet(  
        modifier = modifier,  
        onDismissRequest = closeSheet,  
        sheetState = sheetState,  
        containerColor = Color.White,  
    ) {  
        Column(  
            modifier = Modifier  
                .fillMaxSize()  
                .padding(horizontal = 32.dp),  
            horizontalAlignment = Alignment.CenterHorizontally  
        ) {  
            NonScaleText(  
                "대회 정보",  
                fontSize = 28.sp  
            )  
            HorizontalDivider(color = Color(0x4D252525))  
            NonScaleText(  
                "리워드",  
                fontSize = 12.sp  
            )  
            NonScaleText(  
                "선정 방식",  
                fontSize = 12.sp  
            )  
            NonScaleText(  
                "참여 횟수 제한",  
                fontSize = 12.sp  
            )  
        }  
    }
}  
```


```kotlin
@Composable  
@ExperimentalMaterial3Api  
fun rememberModalBottomSheetState(  
    skipPartiallyExpanded: Boolean = false,  
    confirmValueChange: (SheetValue) -> Boolean = { true },  
) =  
    rememberSheetState(  
        skipPartiallyExpanded = skipPartiallyExpanded,  
        confirmValueChange = confirmValueChange,  
        initialValue = Hidden,  
    )
```
rememberModalBottomSheetState에서 initialValue를 설정할 수 없기 때문에

```kotlin
@OptIn(ExperimentalMaterial3Api::class)  
@DevicePreviews  
@Composable  
private fun HomeScreenBottomSheetPreview() {  
    val sheetState = SheetState(  
        skipPartiallyExpanded = false,  
        initialValue = SheetValue.Expanded,  
        density = LocalDensity.current,  
        skipHiddenState = false  
    )  
  
    HomeScreenBottomSheet(sheetState = sheetState) {}  
}

```
직접 SheetState에서 설정해주어 파라미터로 넘겨주었다.