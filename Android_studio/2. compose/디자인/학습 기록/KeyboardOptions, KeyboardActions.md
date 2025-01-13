[참고 블로그](https://medium.com/@thomas.bernard.310/jetpack-compose-keyboard-cheat-sheet-c3107070e005)

KeyboardOptions.Default.copy(
	keyBoardType
	imeAction
)


imeAction properties
1. ImeAction.Search - 검색 실행.
2. ImeAction.Send  - 입력란 텍스트 보낼 때.
3. ImeAction.Go - 입력한 텍스트 대상 이동.
4. ImeAction.Next - 현재 입력 완료, 다음 텍스트 상자 이동.
5. ImeAction.Done - 입력 완료.


- keyboardActions
```kotlin

// 1번
val keyboardController = LocalSoftwareKeyboardController.current
// 2번
val focusManager = LocalFocusManager.current

BasicTextField(  
    value = value,  
    onValueChange = { newValue ->  
        onValueChange(newValue)  
    },  
    keyboardOptions = KeyboardOptions.Default.copy(imeAction = ImeAction.Done),  
    keyboardActions = KeyboardActions(
	    onDone = { 
		    keyboardController?.hide() // keyboard만 hide
			focusManager.clearFocus()  // keyboard hide + textfield 내 focus 제거
		}  
    ),
    decorationBox = { innerTextField ->  
            innerTextField()  
    }
)
```
