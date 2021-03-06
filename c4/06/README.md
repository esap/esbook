# 4.6 将数字翻译成英文
需求：外贸发票中需要将数字金额翻译成英文
	例如：120.00 翻译为 One Hundred Twenty Dollars

![](./4.6.jpg)

代码
```vb
Function SpellNumber(ByVal MYNUMBER)
    Dim DOLLARS, CENTS, TEMP
    Dim DECIMALPLACE, COUNT
    ReDim PLACE(9) As String
    Application.Volatile True
    PLACE(2) = " THOUSAND "
    PLACE(3) = " MILLION "
    PLACE(4) = " BILLION "
    PLACE(5) = " TRILLION "     ' STRING REPRESENTATION OF AMOUNT
    MYNUMBER = Trim(Str(MYNUMBER))     ' POSITION OF DECIMAL PLACE 0 IF NONE
    DECIMALPLACE = InStr(MYNUMBER, ".")
    'CONVERT CENTS AND SET MYNUMBER TO DOLLAR AMOUNT
    If DECIMALPLACE > 0 Then
        CENTS = GETTENS(Left(Mid(MYNUMBER, DECIMALPLACE + 1) & "00", 2))
        MYNUMBER = Trim(Left(MYNUMBER, DECIMALPLACE - 1))
        End If
    COUNT = 1
    Do While MYNUMBER <> ""
       TEMP = GETHUNDREDS(Right(MYNUMBER, 3))
       If TEMP <> "" Then DOLLARS = TEMP & PLACE(COUNT) & DOLLARS
        	If Len(MYNUMBER) > 3 Then
            	MYNUMBER = Left(MYNUMBER, Len(MYNUMBER) - 3)
            Else
            	MYNUMBER = ""
            End If
			COUNT = COUNT + 1
            Loop
    Select Case DOLLARS
        Case ""
            DOLLARS = "NO DOLLARS"
        Case "ONE"
            DOLLARS = "ONE DOLLAR"
        Case Else
            DOLLARS = DOLLARS & " Dollars"
    End Select
    Select Case CENTS
        Case ""
            CENTS = " AND NO CENTS"
        Case "ONE"
            CENTS = " AND ONE CENT"
        Case Else
            CENTS = " AND " & CENTS & " Cents"
    End Select
    SpellNumber = DOLLARS & CENTS & " Only"
    
End Function
'*******************************************
' CONVERTS A NUMBER FROM 100-999 INTO TEXT *
'*******************************************
Function GETHUNDREDS(ByVal MYNUMBER)
    Dim RESULT As String
    If Val(MYNUMBER) = 0 Then Exit Function
    MYNUMBER = Right("000" & MYNUMBER, 3)     'CONVERT THE HUNDREDS PLACE
    If Mid(MYNUMBER, 1, 1) <> "0" Then
        RESULT = GETDIGIT(Mid(MYNUMBER, 1, 1)) & " HUNDRED "
    End If
    'CONVERT THE TENS AND ONES PLACE
    If Mid(MYNUMBER, 2, 1) <> "0" Then
        RESULT = RESULT & GETTENS(Mid(MYNUMBER, 2))
        Else
        RESULT = RESULT & GETDIGIT(Mid(MYNUMBER, 3))
    End If
    GETHUNDREDS = RESULT
End Function
'*********************************************
' CONVERTS A NUMBER FROM 10 TO 99 INTO TEXT. *
'*********************************************
Function GETTENS(TENSTEXT)
	Dim RESULT As String
	RESULT = ""           'NULL OUT THE TEMPORARY FUNCTION VALUE
	If Val(Left(TENSTEXT, 1)) = 1 Then   ' IF VALUE BETWEEN 10-19
	    Select Case Val(TENSTEXT)
	    	Case 10: RESULT = "TEN"
	        Case 11: RESULT = "ELEVEN"
	        Case 12: RESULT = "TWELVE"
	        Case 13: RESULT = "THIRTEEN"
	        Case 14: RESULT = "FOURTEEN"
	        Case 15: RESULT = "FIFTEEN"
	        Case 16: RESULT = "SIXTEEN"
	        Case 17: RESULT = "SEVENTEEN"
	        Case 18: RESULT = "EIGHTEEN"
	        Case 19: RESULT = "NINETEEN"
	        Case Else
	    End Select
	Else                                 ' IF VALUE BETWEEN 20-99
	    Select Case Val(Left(TENSTEXT, 1))
	        Case 2: RESULT = "TWENTY "
	        Case 3: RESULT = "THIRTY "
	        Case 4: RESULT = "FORTY "
	        Case 5: RESULT = "FIFTY "
	        Case 6: RESULT = "SIXTY "
	        Case 7: RESULT = "SEVENTY "
	        Case 8: RESULT = "EIGHTY "
	        Case 9: RESULT = "NINETY "
	        Case Else
	    End Select
	    RESULT = RESULT & GETDIGIT _
	        (Right(TENSTEXT, 1))  'RETRIEVE ONES PLACE
	End If
	GETTENS = RESULT
End Function
'*******************************************
' CONVERTS A NUMBER FROM 1 TO 9 INTO TEXT. *
'*******************************************
Function GETDIGIT(DIGIT)
    Select Case Val(DIGIT)
        Case 1: GETDIGIT = "ONE"
        Case 2: GETDIGIT = "TWO"
        Case 3: GETDIGIT = "THREE"
        Case 4: GETDIGIT = "FOUR"
        Case 5: GETDIGIT = "FIVE"
        Case 6: GETDIGIT = "SIX"
        Case 7: GETDIGIT = "SEVEN"
        Case 8: GETDIGIT = "EIGHT"
        Case 9: GETDIGIT = "NINE"
        Case Else: GETDIGIT = ""
    End Select
End Function
```
