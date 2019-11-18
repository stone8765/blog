---
layout:	post
title:	.NET Core OCR
author: StoneLi
description: .NET Core OCR
tags: [dotnet, dotnetcore, OCR]
type: dotnet
---

> 本片文章主要讲解.NET 下的OCR取词功能，基于Tesseract库


1. 通过Nuget安装Tesseract类库
2. 下载语言包 https://github.com/tesseract-ocr/tessdata
3. Demo: https://github.com/stone8765/dotnetcore/tree/master/OCR_Test

```csharp
static void Main(string[] args)
{
    var testImage = AppDomain.CurrentDomain.BaseDirectory + "test_image\\a.png";
    try
    {
        using (var engine =new TesseractEngine(@"tessdata","eng+chi_sim",EngineMode.Default))
        {
            using (var img = Pix.LoadFromFile(testImage))
            {
                using (var page = engine.Process(img))
                {
                    var texto = page.GetText();
                    Console.WriteLine(":{0}", page.GetMeanConfidence());
                    Console.WriteLine("Texto:{0}", texto);
                }
                    
            }
        }
    }
    catch(Exception ex)
    {
        Console.WriteLine(ex.Message);
    }
    Console.Read();
}
```

