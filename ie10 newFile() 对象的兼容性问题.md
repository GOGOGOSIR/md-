# ie10 newFile() 对象的兼容性问题

由于ie10 对new File() 的操作会报一个，不可操作的对象的错误

所以我们在做图片上传的时候将base64转化成file对象就会出错

```
export function base64ToFile(dataUrl) {
  let arr = dataUrl.split(',')
  let type = arr[0].match(/:(.*?);/)[1]
  let fileExt = type.split('/')[1]
  let baseStr = window.atob(arr[1])
  let n = baseStr.length
  let u8arr = new Uint8Array(n)
  while(n--) {
      u8arr[n] = baseStr.charCodeAt(n)
  }
  return new File([u8arr],"fileName."+fileExt,{type:type})
}


put() {
        let _this = this
        this.$refs.cropper.getCropData((data) => {
          let file = _this.dataURLtoFile(data, '321.png')
          let param = new FormData() // 创建form对象
          param.append('pic', file) // 通过append向form对象添加数据
          param.append('token', localStorage.token) // 添加form表单中其他数据
          param.append('phone', localStorage.phone) // 添加form表单中其他数据
          param.append('id', JSON.parse(localStorage.u_info).id) // 添加form表单中其他数据=
          let config = {
            headers: {
              'Content-Type': 'multipart/form-data'
            } // 添加请求头
          }
          axios.post(`${baseUrl}/manage/user/addHeaderPics`, param, config)
            .then(res => {
              if(res.data.code === 0) {
                this.$message({
                  type: 'success',
                  message: '图像修改成功'
                })
                _this.img_url = Url.url.$img + res.data.rel.head_pic
                localStorage.img_url = _this.img_url
              } else {
                this.$message({
                  type: 'error',
                  message: '图像修改失败，请重试'
                })
              }
              let file = document.querySelector('#file')
              file.value = ''
              this.dialogVisible = false
            })
        })
      },
```

改进 我们转换成Blob对象

```
dataURLtoFile(dataurl, filename) {
        var arr = dataurl.split(','), mime = arr[0].match(/:(.*?);/)[1],
            bstr = atob(arr[1]), n = bstr.length, u8arr = new Uint8Array(n);
        while (n--) {
            u8arr[n] = bstr.charCodeAt(n);
        }
        return new Blob([u8arr], { type: mime });
},
在做上传操作
put() {
        let _this = this
        this.$refs.cropper.getCropData((data) => {
          let file = _this.dataURLtoFile(data, '321.png')
          let param = new FormData() // 创建form对象
          param.append('pic', file) // 通过append向form对象添加数据
          param.append('token', localStorage.token) // 添加form表单中其他数据
          param.append('phone', localStorage.phone) // 添加form表单中其他数据
          param.append('id', JSON.parse(localStorage.u_info).id) // 添加form表单中其他数据=
          let config = {
            headers: {
              'Content-Type': 'multipart/form-data'
            } // 添加请求头
          }
          axios.post(`${baseUrl}/manage/user/addHeaderPics`, param, config)
            .then(res => {
              if(res.data.code === 0) {
                this.$message({
                  type: 'success',
                  message: '图像修改成功'
                })
                _this.img_url = Url.url.$img + res.data.rel.head_pic
                localStorage.img_url = _this.img_url
              } else {
                this.$message({
                  type: 'error',
                  message: '图像修改失败，请重试'
                })
              }
              let file = document.querySelector('#file')
              file.value = ''
              this.dialogVisible = false
            })
        })
      },
```