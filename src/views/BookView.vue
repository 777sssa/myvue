<!--<template>-->
<!--  <div>-->
<!--    <el-input-->
<!--        placeholder="请输入书名"-->
<!--        v-model="searchQuery"-->
<!--        clearable-->
<!--        @clear="handleSearch"-->
<!--        @keyup.enter="handleSearch"-->
<!--        style="margin-bottom: 20px;"-->
<!--    />-->
<!--    <table>-->
<!--      <tr>-->
<!--        <td>编号</td>-->
<!--        <td>图书名称</td>-->
<!--        <td>作者</td>-->
<!--      </tr>-->
<!--      <tr v-for="item in books">-->
<!--        <td>{{item.id}}</td>-->
<!--        <td>{{item.name}}</td>-->
<!--        <td>{{item.author}}</td>-->
<!--      </tr>-->
<!--    </table>-->
<!--  </div>-->
<!--</template>-->

<!--<script>-->

<!--import axios from 'axios'-->
<!--export default {-->
<!--   name:"Book",-->
<!--   data(){-->
<!--     return {-->
<!--       books:[]-->
<!--     }-->
<!--   },-->
<!--   created(){-->
<!--     const _this = this-->
<!--     axios.get( 'http://localhost:8181/book/findAll').then(function(resp){-->
<!--       ///console.log(resp.data)-->
<!--       _this.books = resp.data-->
<!--     })-->


<!--   }-->
<!-- }-->


<!--</script>-->

<template>
  <div>
    <input v-model="searchQuery" placeholder="搜索图书名称" @input="searchBooks" />
    <table>
      <thead>
      <tr>
        <th>编号</th>
        <th>图书名称</th>
        <th>作者</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="book in books" :key="book.id">
        <td>{{ book.id }}</td>
        <td>{{ book.name }}</td>
        <td>{{ book.author }}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      searchQuery: '',
      books: []
    };
  },
  methods: {
    async searchBooks() {
      try {
        const response = await axios.get(`http://localhost:8181/book/findByName`, {
          params: {
            name: this.searchQuery
          }
        });
        this.books = response.data;
      } catch (error) {
        console.error('获取书籍数据出错:', error);
      }
    }
  }
};

</script>
