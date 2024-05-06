# VBA-Challenge-
Sub VBA_analysis()
    
    Dim Current As Worksheet
    
    For Each Current In Worksheets
        
        With Current
            
            Dim Ticker_Name As String
            
            Dim Ticker_Total_Volume As Double
            
            Ticker_Total_Volume = 0
            
            Dim Summary_Table_Row As Integer
            
            Summary_Table_Row = 2
            
            .Range("I1").Value = "Ticker"
            .Range("J1").Value = "Yearly Change"
            .Range("K1").Value = "Percent Change"
            .Range("L1").Value = "Total Stock Volume"
            
            Dim Row_Count As Long
            
            Row_Count = .Cells(.Rows.Count, 1).End(xlUp).Row
            
            Dim First_Open As Double
            
            Dim Last_Close As Double
            
            For i = 2 To Row_Count
                
                Dim val1 As String
                
                Dim val2 As String
                
                val1 = .Cells(i + 1, 1).Value
                
                val2 = .Cells(i, 1).Value
                
                If i = 2 Then
                    
                    First_Open = .Cells(i, 3).Value
                
                End If
                
                If val1 <> val2 Then
                    
                    Ticker_Name = .Cells(i, 1).Value
                    
                    Ticker_Total_Volume = Ticker_Total_Volume + .Cells(i, 7).Value
                    Last_Close = .Cells(i, 6).Value
                    
                    .Range("I" & Summary_Table_Row).Value = Ticker_Name
                    
                    .Range("J" & Summary_Table_Row).Value = Last_Close - First_Open
                    
                    If (First_Open <> 0) Then
                        
                        .Range("K" & Summary_Table_Row).Value = ((Last_Close - First_Open) / First_Open) * 100
                    
                    End If
                    
                    .Range("L" & Summary_Table_Row).Value = Ticker_Total_Volume
                    
                    Summary_Table_Row = Summary_Table_Row + 1
                    
                    Ticker_Total_Volume = 0
                    
                    First_Open = .Cells(i + 1, 3)
                
                Else
                    
                    Ticker_Total_Volume = Ticker_Total_Volume + .Cells(i, 7).Value
                
                End If
            
            Next i
            
            Dim Fmt_Range As Range
            
            Set Fmt_Range = .Range("J2:J" & CStr(Summary_Table_Row - 1))
            
            Fmt_Range.FormatConditions.Delete
            
            Fmt_Range.FormatConditions.Add Type:=xlCellValue, Operator:=xlLess, Formula1:="=0"
            
            Fmt_Range.FormatConditions(1).Interior.Color = RGB(255, 0, 0)
            
            Fmt_Range.FormatConditions.Add Type:=xlCellValue, Operator:=xlGreaterEqual, Formula1:="=0"
            
            Fmt_Range.FormatConditions(2).Interior.Color = RGB(0, 255, 0)
        
        End With
    
    Next Current

End Sub
