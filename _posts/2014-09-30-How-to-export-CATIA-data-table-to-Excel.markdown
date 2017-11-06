---
layout:     post
title:      "How to export CATIA data table to Excel"
subtitle:   "With a handy VisualBasic script you can make your life easier."
date:       2014-09-30 12:00:00
author:     "z"
header-img: "img/curves.jpg"
---

<p>Just use the following VB-Script in CATIA VB Editor (Alt+F11), and it will do everything for You:
<pre><code>
Sub TableToExcel()
'\\Global Declarations
Dim bigstring
Dim totalrows
Dim totalcolumns
Dim drwdoc As Document
Dim drwsheets As DrawingSheets
Dim drwsheet As DrawingSheet
Dim drwviews As DrawingViews
Dim drwview As DrawingView
Dim drwtables As DrawingTables
Dim drwtable As DrawingTable
Dim objExcel As Object
Dim objWorkbook
Dim objSheet
Dim objCell
Dim InputObjectType(0)
Dim i, j, k, r, c
Dim IsExcelRunning As Boolean
Set drwdoc = CATIA.ActiveDocument
If TypeName(drwdoc) <> "DrawingDocument" Then
MsgBox "This macro can only be run with a drawing document.", vbInformation, "Document not a Drawing"
Exit Sub
End If
Set drwsheets = drwdoc.Sheets
'Here I capture whether MS Excel is running already or not so we don't close it on the user
On Error Resume Next
Set objExcel = GetObject(, "Excel.Application")
If objExcel Is Nothing Then
IsExcelRunning = False 'false if user does not have excel running
Err.Clear
Else
IsExcelRunning = True 'true if the user has excel running
End If
On Error GoTo 0
'run thru every sheet
For i = 1 To drwsheets.Count
Set drwsheet = drwsheets.Item(i)
Set drwviews = drwsheet.Views
'run thru every drawing view
For j = 1 To drwviews.Count
Set drwview = drwviews.Item(j)
Set drwtables = drwview.Tables
'run thru Tables
If drwtables.Count > 0 Then
For k = 1 To drwtables.Count
Set drwtable = drwtables.Item(k)
'--Define table and extract strings to array.
totalrows = drwtable.NumberOfRows
totalcolumns = drwtable.NumberOfColumns
Dim table()
ReDim table(totalrows, totalcolumns)
For r = 1 To totalrows
For c = 1 To totalcolumns
table(r, c) = drwtable.GetCellString(r, c)
Next
Next
'--Open Excel
On Error Resume Next
Set objExcel = GetObject(, "Excel.Application")
If objExcel Is Nothing Then
Set objExcel = CreateObject("Excel.Application")
Err.Clear
objExcel.Visible = False
End If
On Error GoTo 0
Dim myname
myname = Left(drwdoc.FullName, Len(drwdoc.FullName) - 11) & "_" & drwsheet.Name & "_" & drwview.Name & "_" & drwtable.Name & ".xls"
Set objWorkbook = objExcel.Workbooks.Add()
objWorkbook.Activate
Set objSheet = objWorkbook.Worksheets.Item(1)
'--Populate spreadsheet with values from array.
For r = 1 To totalrows
For c = 1 To totalcolumns
objSheet.Cells(r, c) = table(r, c)
Next
Next
'--objWorkbook.SaveAs myname 'ExcelWorksheet.SaveAs sFile
'--objWorkbook.Close
Next k ' table loop
End If
Next j ' view loop
Next i ' Sheet loop
If IsExcelRunning = False Then
If objExcel Is Nothing Then
MsgBox "There are no Drawing tables to exprot.", vbInformation, "No Tables to export"
Exit Sub
End If
objExcel.Quit ' Close Excel with the Quit method on the Application object
MsgBox "Your Catia drawing tables have been exported to Excel.", vbInformation, "Table Export Complete"
End If
End Sub

</pre></code></p>
