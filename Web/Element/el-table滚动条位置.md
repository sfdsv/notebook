1.设置table的ref为tableList
 
2.设置滚动至顶部

```vue
this.$refs.tableList.bodyWrapper.scrollTop =0;
```

3.设置滚动至底部

```vue
this.$refs.tableList.bodyWrapper.scrollTop =this.$refs.tableList.bodyWrapper.scrollHeight;
```
