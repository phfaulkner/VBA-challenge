# The VBA of Wall Street
 VBA Homework

VBA Challenges
Create the VBA scripting to analyze stock market data for 3 consecutive years (2018-2020).

Created Github repository to load the files into.

Challenge 2
Add the VBA files for the assignment to the Github repository

Challenge 3
Create a script that loops through all the stocks for one year and outputs the following information:
  * The ticker symbol.
  * Yearly change from opening price at the beginning of a given year to the closing price at the end of that year.
  * The percent change from opening price at the beginning of a given year to the closing price at the end of that year.
  * The total stock volume of the stock.
The final product should use conditional formatting that will highlight positive change in green and negative change in red.

Below is the code for this project and I have included prints of the results in the repository as well.


VBA Code for Wall Street
Sub MultipleYearStockData():
    For Each ws In Worksheets
    
        Dim WorksheetName As String
        'Current row
        Dim i As Long
        'Start row of ticker block
        Dim j As Long
        'Index counter to fill Ticker row
        Dim TickCount As Long
        'Last row column A
        Dim LastRowA As Long
        'last row column I
        Dim LastRowI As Long
        'Variable for percent change calculation
        Dim PerChange As Double
        'Variable for greatest increase calculation
        Dim GreatIncr As Double
        'Variable for greatest decrease calculation
        Dim GreatDecr As Double
        'Variable for greatest total volume
        Dim GreatVol As Double

        'Get the WorksheetName
        'Get the WorksheetName
        WorksheetName = ws.Name

        'Create column headers
        ws.Cells(1, 9).Value = "Ticker"
        ws.Cells(1, 10).Value = "Yearly Change"
        ws.Cells(1, 11).Value = "Percent Change"
        ws.Cells(1, 12).Value = "Total Stock Volume"
        ws.Cells(1, 16).Value = "Ticker"
        ws.Cells(1, 17).Value = "Value"
        ws.Cells(2, 15).Value = "Greatest % Increase"
        ws.Cells(3, 15).Value = "Greatest % Decrease"
        ws.Cells(4, 15).Value = "Greatest Total Volume"
        
        'Set The Ticker Counter in the First Row
        TickCount = 2
        
        'Set start row to 2
        j = 2
        
        'Locate the last non-blank cell in column A
        LastRowA = ws.Cells(Rows.Count, 1).End(xlUp).Row
        'MsgBox ("Last row in column A is " & LastRowA)
        
            'Loop through all rows
            For i = 2 To LastRowA
            
                'Check if ticker name changed
                If ws.Cells(i + 1, 1).Value <> ws.Cells(i, 1).Value Then
                
                'Write ticker in column I (#9)
                ws.Cells(TickCount, 9).Value = ws.Cells(i, 1).Value
                
                'Calculate and write Yearly Change in column J (#10)
                ws.Cells(TickCount, 10).Value = ws.Cells(i, 6).Value - ws.Cells(j, 3).Value
                
                    'Conditional formating
                    If ws.Cells(TickCount, 10).Value < 0 Then
                
                    'Set cell background color to red
                    ws.Cells(TickCount, 10).Interior.ColorIndex = 3
                
                    Else
                
                    'Set cell background color to green
                    ws.Cells(TickCount, 10).Interior.ColorIndex = 4
                
                    End If
                    
                    'Calculate and write percent change in column K (#11)
                    If ws.Cells(j, 3).Value <> 0 Then
                    PerChange = ((ws.Cells(i, 6).Value - ws.Cells(j, 3).Value) / ws.Cells(j, 3).Value)
                    
                    'Percent formating
                    ws.Cells(TickCount, 11).Value = Format(PerChange, "Percent")
                    
                    Else
                    
                    ws.Cells(TickCount, 11).Value = Format(0, "Percent")
                    
                    End If
                    
                'Calculate and write total volume in column L (#12)
                ws.Cells(TickCount, 12).Value = WorksheetFunction.Sum(Range(ws.Cells(j, 7), ws.Cells(i, 7)))
                
                'Increase TickCount by 1
                TickCount = TickCount + 1
                
                'Set new start row of the ticker block
                j = i + 1
                
                End If
            
            Next i
            
        'Locate last non-blank cell in column I
        LastRowI = ws.Cells(Rows.Count, 9).End(xlUp).Row
        'MsgBox ("Last row in column I is " & LastRowI)
        
        'Prepare for the summary
        GreatVol = ws.Cells(2, 12).Value
        GreatIncr = ws.Cells(2, 11).Value
        GreatDecr = ws.Cells(2, 11).Value
        
            'Loop for the summary
            For i = 2 To LastRowI
            
                'For the greatest total volume--check if the next value is larger--if yes, take over to cell a new value and then populate ws.Cells
                If ws.Cells(i, 12).Value > GreatVol Then
                GreatVol = ws.Cells(i, 12).Value
                ws.Cells(4, 16).Value = ws.Cells(i, 9).Value
                
                Else
                
                GreatVol = GreatVol
                
                End If
                
                'For greatest increase--check if next value is larger and if yes, take over a new value to populate ws.Cells
                If ws.Cells(i, 11).Value > GreatIncr Then
                GreatIncr = ws.Cells(i, 11).Value
                ws.Cells(2, 16).Value = ws.Cells(i, 9).Value
                
                Else
                
                GreatIncr = GreatIncr
                
                End If
                
                'For greatest decrease--check if next value is smaller and if yes, take over a new value to populate ws.Cells
                If ws.Cells(i, 11).Value < GreatDecr Then
                GreatDecr = ws.Cells(i, 11).Value
                ws.Cells(3, 16).Value = ws.Cells(i, 9).Value
                
                Else
                
                GreatDecr = GreatDecr
                
                End If
                
            'Write summary results in ws.Cells
            ws.Cells(2, 17).Value = Format(GreatIncr, "Percent")
            ws.Cells(3, 17).Value = Format(GreatDecr, "Percent")
            ws.Cells(4, 17).Value = Format(GreatVol, "Scientific")
            
            Next i
            
        'Djust column width automatically
        Worksheets(WorksheetName).Columns("A:Z").AutoFit
            
    Next ws
        
End Sub

![Alt text]("C:\Users\Owner\OneDrive\Documents (OneDrive)\Training\Ga Tech Files Working Files\HomeWork\02-VBA Homework\Instructions\2018_Wall_Street_Pic.png")




