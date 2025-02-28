- 开明式标点是指句末标点全宽，句内标点半宽。[^开明式标点]
  句内标点有逗号、顿号、分号、冒号（`，` `、` `：` `；`）和引号、括号、书名号（`“”` `‘’` `『』` `「」` `（ ）` `《 》` `〈 〉`）[^标点符号用法]。
  
  [^开明式标点]: https://www.thetype.com/2018/02/14211/
  [^标点符号用法]: https://news.sjtu.edu.cn/zhxw/20180405/40801.html
- ```vb
  Sub AdjustSpecificCharacterSpacing()
      Dim para As Paragraph
      Dim rng As Range
      Dim fontSize As Single
      Dim spacingAdjustment As Single
      Dim char As String
      Dim prevChar As String
      Dim nextChar As String
      Dim leftSymbols As String
      Dim rightSymbols As String
      Dim indentation As Single
      
      ' 需要处理的符号集
      leftSymbols = "“‘『「（《〈"
      rightSymbols = "”’』」）》〉"
      
      ' 遍历文档中的所有段落
      For Each para In ActiveDocument.Paragraphs
          ' 获取段落的 Range
          Set rng = para.Range
          
          ' 遍历 Range 中的每个字符
          For i = 1 To rng.Characters.Count
              char = rng.Characters(i)
              prevChar = ""
              nextChar = ""
              
              ' 获取当前字符前一个字符和后一个字符
              If i > 1 Then
                  prevChar = rng.Characters(i - 1)
              End If
              If i < rng.Characters.Count Then
                  nextChar = rng.Characters(i + 1)
              End If
              
              fontSize = rng.Characters(i).Font.Size
              
              ' 处理左半边符号：“‘『「（《〈
              If InStr(leftSymbols, char) > 0 Then
                  ' 如果前一个字符是汉字，则缩进前一个汉字
                  If prevChar Like "[一-龥]" Then
                      ' 根据字号设置缩进值
                      indentation = GetSpacingAdjustment(fontSize)
                      
                      ' 对前一个汉字进行缩进
                      rng.Characters(i - 1).Font.Spacing = indentation
                  End If
              End If
              
              ' 处理右半边符号：”’』」）》〉
              If InStr(rightSymbols, char) > 0 Then
                  ' 如果后一个字符是汉字，则调整当前符号字符的缩进
                  If nextChar Like "[一-龥]" Then
                      ' 根据字号设置缩进值
                      indentation = GetSpacingAdjustment(fontSize)
                      
                      ' 对当前符号进行缩进
                      rng.Characters(i).Font.Spacing = indentation
                  End If
              End If
              
              ' 对于特定字符的字符间距调整：，、；：等
              If InStr("，、；：", char) > 0 Then
                  ' 获取字符间距调整值
                  spacingAdjustment = GetSpacingAdjustment(fontSize)
                  
                  ' 调整字符间距
                  If spacingAdjustment <> 0 Then
                      rng.Characters(i).Font.Spacing = spacingAdjustment
                  End If
              End If
          Next i
      Next para
  End Sub
  
  ' 子方法：根据字号返回对应的字符间距调整值
  Function GetSpacingAdjustment(fontSize As Single) As Single
      Select Case fontSize
          Case 10.5 ' 五号字
              GetSpacingAdjustment = -1.8 ' 五号字缩小 1.8pt
          Case 12 ' 小四号字
              GetSpacingAdjustment = -2.1 ' 小四号字缩小 2.1pt
          Case 14 ' 四号字
              GetSpacingAdjustment = -2.3 ' 四号字缩小 2.3pt
          Case 16 ' 三号字
              GetSpacingAdjustment = -2.6 ' 三号字缩小 2.6pt
          Case 22 ' 二号字
              GetSpacingAdjustment = -3.7 ' 二号字缩小 3.7pt
          Case Else
              GetSpacingAdjustment = 0 ' 如果不是目标字号，则不缩进
      End Select
  End Function
  ```