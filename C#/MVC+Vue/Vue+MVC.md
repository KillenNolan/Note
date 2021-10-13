# Vue+MVC



将vue当做一个库来用

在前端Html页面中，标签引入

<script src="https://cdn.jsdelivr.net/npm/vue@2.6.13/dist/vue.js" type="text/javascript"></script>

```



<ul id="v-for-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      title: 'How to do lists in Vue',
      author: 'Jane Doe',
      publishedAt: '2016-04-10'
    }
  }
})
```