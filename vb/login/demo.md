# 登入三次鎖定
1. 建立資料表，例如`LoginErr`  
   ```sql
    CREATE TABLE [dbo].[LoginErr](
	[id] [int] IDENTITY(1,1) NOT NULL,
	[userid] [varchar](50) NOT NULL,
	[errCount] [int] NOT NULL,
	[errDate] [datetime] NOT NULL,
	[enable] [bit] NOT NULL,
    CONSTRAINT [PK_LoginErr] PRIMARY KEY CLUSTERED 
    (
        [id] ASC
    )WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
    ) ON [PRIMARY]
   ```  
2. 加入兩個函式  
   ```vb
   Public Shared Sub SetLoginState(ByVal userid As String, ByVal success As Boolean)
        Dim db As New StudentPortfolioDataContext
        Dim result = db.LoginErr.Where(Function(c) c.userid = userid And c.enable = True).OrderByDescending(Function(o) o.id).FirstOrDefault
        If success = True Then
            If result IsNot Nothing Then
                result.enable = False
            End If
        Else
            If result IsNot Nothing Then
                result.errCount += 1
                result.errDate = Now
            Else
                Dim newResult As New LoginErr With {.userid = userid, .errCount = 1, .errDate = Now, .enable = True}
                db.LoginErr.InsertOnSubmit(newResult)
            End If
        End If
        db.SubmitChanges()
    End Sub

    Public Shared Function IsLoginLock(ByVal userid As String) As Boolean
        Dim db As New StudentPortfolioDataContext
        Dim result = db.LoginErr.Where(Function(c) c.userid = userid And c.enable = True).OrderByDescending(Function(o) o.id).FirstOrDefault
        If result IsNot Nothing Then
            If result.errCount >= 3 AndAlso result.errDate.AddMinutes(30) > Now Then
                Return True
            Else
                Return False
            End If
        Else
            Return False
        End If
    End Function
   ```  
3. 完成!在登入頁登入時檢查  
   ```vb
    If IsLoginLock(Login1.UserName) = True Then            
            Login1.FailureText = "帳號已被鎖定,無法登入"
    Else
        If ldapverify("xxx.xxxx.edu.tw", 389, Login1.UserName, Login1.Password) = "通過密碼檢查" Then                
            PortfolioAPI.SetLoginState(Login1.UserName, True)
            e.Authenticated = True
        Else
            PortfolioAPI.SetLoginState(Login1.UserName, False)                
        End If
    End If
   ```