
> 登陆页面点击第三方登陆 -> 页面重定向GitHub登陆页面-> 点击授权 -> 返回redirect_uri ->  前端获取code -> 向后端发请求->  后端根据code获取access_token -> 根据access_token获取用户信息-> 返回给前端

### 申请一个 OAuth App
1. 登录 github
2. 点击头像下的 Settings -> Developer settings 右侧 New OAuth App
3. 填写申请 app 的相关配置，重点配置项有2个
4. Homepage URL 这是后续需要使用授权的 URL ，你可以理解为就是你的项目根目录地址
5. 点击头像下的 Settings -> Developer settings 右侧 New OAuth App

