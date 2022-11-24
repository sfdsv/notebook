this.$alert:
```vue
this.$alert('这是一段内容', '标题名称', {
                confirmButtonText: '确定',
                callback: action => {
                  this.message({
                    type: 'info',
                    message: 'action:${action}'
                  });
                }
              });
```

this.$prompt:
```vue
this.$prompt(
                '请输入邮箱：',
                '提示', {
                  confirmButtonText: '确定',
                  cancelButtonText: '取消'
                }).then(({value}) => {
                  this.$message({
                    type:'success',
                    message:'你的邮箱是：'+value
                  });
                console.log(value);
                // TO DO DO ...
              }).catch(() => {
                this.$message({
                  type:'info',
                  message:'取消输入'
                });
                // console.log(err);
              });
```

