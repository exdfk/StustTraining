### WEB API POST FORMDATA
1. 打開Models裡的`queryModel.cs`，加入一個`ICollection<IFormFile>`的屬性  
   ```csharp
    namespace DemoCore6.Models;
    using System.ComponentModel.DataAnnotations;
    public class queryModel
    {
        public string? name { get; set; }

        [Range(0, 100, ErrorMessage = "值要介於0~100之間")]
        public int age { get; set; }

        public ICollection<IFormFile>? files { get; set; }
    }
   ```  
2. 打開Controllers裡的`StudControll.cs`，加入一個Post方法  
   ```csharp
    [EnableCors("PublicPolicy")]
    [HttpPost("fileuplaods")]
    public IActionResult PostStudsFiles([FromForm] Models.queryModel stud)
    {       
        var name = stud?.name;
        var age = stud?.age;
        int filesize = 0;
        if (stud?.files != null)
        {
            foreach (var f in stud.files)
            {
                MemoryStream ms = new MemoryStream();
                f.CopyTo(ms);
                var filebytes = ms.ToArray();
                var filename = f.FileName;
                filesize += filebytes.Count();
            }
        }
        return Ok($"name:{name},age:{age},filesize:{filesize}");
    }
   ```  
3. 設計一個html表單，有name有age的input,及一個多檔上傳的input，並加上提交的button按鈕，寫一段javascript測試一下表單及附件的上傳    
   ```html
    <html>
    <head>
        <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
    </head>

    <body>
        <span>name:</span><input id="name" /><br />
        <span>age:</span><input id="age" /><br />
        <input type="file" id="file" multiple /><br />
        <input type="button" onclick="sumbit()" value="提交" />
    </body>
    <script>
        let sumbit = () => {
            let name = document.getElementById("name").value;
            let age = document.getElementById("age").value;
            let files = document.getElementById('file').files;
            let formData = new FormData();
            for (var i = 0; i < files.length; i++)
                formData.append("files", files[i]);
            formData.append("name", name);
            formData.append("age", age);
            axios.post("https://localhost:7049/stud/fileuplaods", formData).then(res => {
                alert(res.data);
            })
        }
    </script>
    </html>
   ```  
   ### [上一頁 WEBAPI POST](post.md)