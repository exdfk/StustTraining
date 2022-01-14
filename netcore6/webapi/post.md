# WEBAPI POST
1. 我們繼續延用目錄Controllers做的`StudControllers.cs`檔，加入程式碼  
   ```csharp
    [EnableCors("PublicPolicy")]
    [HttpPost]
    public IActionResult PostStud([FromBody] Models.queryModel stud)
    {
        return Ok($"{stud.name},新增完成!");
    }
   ```  
2. 將專案啟動執行，寫一段javascript試著把資料用post傳遞至webapi  
   ```html
    <script>
        let stud = { name: 'airmanx', age: 18 };
        axios.post("https://localhost:7049/stud", stud).then(res => {
            alert(res.data);
        })
    </script>
   ```  
3. 接下來思考看看，如果要post多筆資料應如何處理？`StudControllers.cs`檔，加入程式碼  
   ```csharp
    [EnableCors("PublicPolicy")]
    [HttpPost("import")]
    public IActionResult PostStuds([FromBody] List<Models.queryModel> studs)
    {
        return Ok($"共匯入{studs.Count()}筆資料");
    }
   ```  
4. javascript  
   ```html
    <script>
        let studs = [{ name: 'airmanx', age: 18 }, { name: 'ccsu', age: 55 }];
        axios.post("https://localhost:7049/stud/import", studs).then(res => {
            alert(res.data);
        })
    </script>
   ```  
   ### [上一頁 DataAnnotations](dataAnnotations.md)  
   ### [下一頁 WEB API POST FORMDATA](post2.md)