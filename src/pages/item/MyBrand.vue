<template>
    <div>
      <v-layout class="px-3 pb-3" ><!--保存于同一行   px:即padding左和右-->
        <v-flex xs2> <!--占两格-->
          <v-btn color="info">新增品牌</v-btn>
        </v-flex>
        <v-spacer/><!--撑开-->
        <v-flex xs4><!--占4格-->  <!--hide-details 隐藏细节 致使间隙变窄-->
          <v-text-field
            label="搜索" hide-details append-icon="search" v-model="key"
          ></v-text-field>
        </v-flex>
      </v-layout>



      <v-data-table
        :headers="headers"
        :items="brands"
        :pagination.sync="pagination"
        :server-items-length="totalBrands"
        :loading="loading"
        class="elevation-1"
      >
        <template slot="items" slot-scope="props">
          <td class="text-xs-right">{{ props.item.id }}</td>
          <td class="text-xs-center">{{ props.item.name }}</td>
          <!--<td class="text-xs-center">{{ props.item.image }}</td>-->
          <td class="text-xs-center"><img :src="props.item.image" alt="哈哈哈"></td><!-- :src 变量-->
          <td class="text-xs-center">{{ props.item.letter }}</td>
          <td class="text-xs-center">
            <v-btn text icon color="info">
              <v-icon>edit</v-icon>
            </v-btn>
            <v-btn text icon color="error">
              <v-icon>delete</v-icon>
            </v-btn>
            <!--<v-btn  color="modified" small>修改</v-btn>-->
            <!--<v-btn  color="delete" small>删除</v-btn>-->
          </td>
        </template>


      </v-data-table>
    </div>
</template>


<script>
    export default {
        name: "MyBrand",
        data(){
          return{
            headers:[
              {
                text: '品牌id',
                value: 'id',
                align:  'center' ,
                sortable: true,
              },
              {
                text: '品牌名称',
                value: 'name',
                align:  'center' ,
                sortable: false,
              },
              {
                text: '品牌LOGO',
                value: 'image',
                align:  'center' ,
                sortable: false,
              },
              {
                text: '品牌首字母',
                value: 'letter',
                align:  'center' ,
                sortable: true,
              },
              { text: '操作',align:  'center' ,sortable: false, },

            ],
            brands:[]// 数据查询出来  created() 页面创建即加载
            ,pagination:{},
            totalBrands: 0,
            loading: false,
            key:""//搜索条件
          }
      },
        created() {
          this.brands = [
            {id:1,name:"mi",image: "1.jpg",letter: "m"},
            {id:11,name:"ximi",image: "11.jpg",letter: "x"},
          ];
          this.totalBrands = 15;

            // 查数据
          this.loadBrands();
        },

      //实时监控   此处为搜索条件实时反应
      watch:{
        key(){
          this.pagination.page = 1;//搜索条件变更都从第一页开始
          // this.loadBrands();
        },
        pagination:{
          deep: true,

          handler(){
            this.loadBrands();
          }
        }
      },

        methods:{
            loadBrands(){
              this.loading = true;//显示加载进度条
              //$http == axios
              // this.$http.get("/brand/page",{
              this.$http.get("/item/brand/page",{
                params:{

                  //分页
                  page: this.pagination.page,//当前页
                  rows: this.pagination.rowsPerPage,//每页大小
                  sortBy: this.pagination.sortBy,//排序字段
                  desc: this.pagination.descending,//是否是降序

                  //搜索
                  key: this.key//搜索框条件
                }
              }).then(resp => {
                console.log(resp);
                  this.brands = resp.data.items;
                  this.totalBrands = resp.data.totals;
                this.loading = false;
              })
            }
        }
    }
</script>

<style scoped>

</style>
