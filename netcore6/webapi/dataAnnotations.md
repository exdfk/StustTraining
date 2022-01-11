# DataAnnotations
1. 打開在Modles目錄的`queryModel.cs`，我們引入`using System.ComponentModel.DataAnnotations;`，以及在每個property加入我們想驗證的項目，列如我們想在name驗証前端是否有插入script，age有範圍限制，程式碼如下    
   ```csharp
    namespace DemoCore6.Models;
    using System.ComponentModel.DataAnnotations;
    public class queryModel
    {
        [RegularExpression(@"|(<(script|iframe|embed|frame|frameset|object|img|applet|body|html|style|layer|link|ilayer|meta|bgsound))",ErrorMessage="xss攻擊")]
        public string? name { get; set; }

        [Range(10, 20, ErrorMessage = "值要介於10~20之間")]
        public int age { get; set; }
    }
   ```  
2. 寫段javascript測試一下  
   ```html
    <script>
        axios.get("https://localhost:7049/stud/query", { params: { name: '<script>alert("hello"</scritp>', age: 50 } })
            .then(res => {
                alert(res.data);
            })
            .catch(err=>{
                alert(Object.entries(err.response.data.errors).join());
            })   
    </script>
   ```  
3. 更多的驗證，請參考<a target="_blank" href="https://geeksarray.com/blog/how-to-validate-mvc-model-using-dataannotation-attributes">How To Validate MVC Model Using DataAnnotation Attribute</a>  
### [上一頁 WEB API GET 傳參](get2.md)
### [下一頁 WEB API POST](post.md)
