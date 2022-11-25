实现效果：  

![image-20221125000728291](https://cdn.jsdelivr.net/gh/sfdsv/picbed/img/202211250007347.png)


实现代码：
```vue
<el-steps :active=values space="400px" align-center>
<el-step :title="item.title" :description="item.date" v-for="(item,index) in stepsData" :key="index">
</el-step>
</el-steps>
       
export default {
        data() {
          return {
            stepsData: [
              {
                title: '启动',
                date: ''
              },
              {
                title: '发布',
                date: ''
              },
              {
                title: '洽谈',
                date: ''
              },
              {
                title: '规划',
                date: ''
              },
              {
                title: '签约',
                date: ''
              },
              {
                title: '落项',
                date: ''
              },
            ],
            values: 3
          }
        }
      }
```

