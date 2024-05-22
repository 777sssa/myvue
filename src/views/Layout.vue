<script>
import Aside from  '../components/Aside.vue'
import axios from 'axios';
export default {
  name: "Layout",
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
  },
  components:{
    Aside
  }
};

</script>

<template>

<!--  header-->
  <el-container class="main-container">

    <el-header class="fixed-header" style="margin-bottom: 0px;">
      <el-row>
        <el-col :span="4"><div class="grid-content bg-purple"><img class="aminer" src="../assets/img.png"></div></el-col>
        <el-col :span="2"><div class="grid-content bg-purple">333</div></el-col>
        <el-col :span="2"><div class="grid-content bg-purple">333</div></el-col>
        <el-col :span="8"><div class="grid-content bg-purple-light" >
          <input
              v-model="searchQuery"
              placeholder="搜索作者名称"
              @input="searchBooks"
              style="width: 80%; height: 20px; padding: 10px; font-size: 16px; border-radius: 5px; border: 1px solid #ccc; margin-bottom: 10px;"
              />
        </div></el-col>
        <el-col :span="4"><div class="grid-content bg-purple">333</div></el-col>

        <el-col :span="1"><div class="grid-content bg-purple" >
          <i class="el-icon-upload" style="color: white;font-size: 30px"></i>
        </div></el-col>
        <el-col :span="1"><div class="grid-content bg-purple">
          <i class="el-icon-message-solid" style="color: white;font-size: 30px"></i>
        </div></el-col>
        <el-col :span="1"><div class="grid-content bg-purple">
          <i class="el-icon-s-tools " style="color: white;font-size: 30px"></i>
        </div></el-col>
        <el-col :span="1"><div class="grid-content bg-purple" style="color: white;">未登录</div></el-col>

      </el-row>
    </el-header>



    <el-container style="margin-top: -20px;margin-left: 20px;margin-right: 20px;">

      <!--    aside-->
      <el-aside width="330px" class="flex-container" style="border-radius: 20px;border-left: 1px dashed lightgray; ">
<!--        左1-->
        <div class="grid-content bg-purple custom-text" style="border-radius: 20px;overflow: hidden;">
<!--part1-->

            <div style="height: 110px; background-color: darkslateblue; display: flex; align-items: center;">
              <div style="flex: 1.5; display: flex; align-items: center; justify-content: flex-end;">
                <!-- 左侧部分 -->
                <img class="aminer" src="../assets/img_1.png" style="width: 60px; height: 60px; object-fit: cover; margin-right: 20px;">
              </div>
              <div style="flex: 3; display: flex; align-items: center; justify-content: flex-start;">
                <!-- 右侧部分 -->
                <div v-for="book in books" :key="book.id">
                  <span style="display: inline-block;color: white;font-weight: bold;">{{ book.name }}</span><br>
                <span style="display: block; color: white;">{{book.position}}</span>
                </div>
              </div>
            </div>

<!--part 2-->
          <div  v-for="book in books" :key="book.id" style="height: 100px; background-color: rgba(199, 216, 230, 0.3);padding-left: 10px;font-size: 14px; color: black; display: flex; align-items: center; justify-content: center;text-align: left;">
              {{book.org}}
          </div>
<!--part 3-->
          <div style="height: 40px; background-color:rgba(199, 216, 230, 0.3); display: flex; justify-content: space-evenly; align-items: center;">
            <i class="el-icon-edit" style="font-size: 20px; color: rebeccapurple"></i>
            <i class="el-icon-message-solid" style="font-size: 20px; color: rebeccapurple"></i>
            <i class="el-icon-delete" style="font-size: 20px; color: rebeccapurple"></i>
            <i class="el-icon-s-promotion" style="font-size: 20px; color: rebeccapurple"></i>
            <i class="el-icon-s-custom" style="font-size: 20px; color: rebeccapurple"></i>
            <i class="el-icon-s-home" style="font-size: 20px; color: rebeccapurple"></i>
          </div>
<!--part 4-->
          <div style="height: 40px; background-color: rgba(199, 216, 230, 0.3);display: flex; justify-content: space-evenly; align-items: center;">
            <i class="el-icon-s-flag" style="font-size: 15px; color: rebeccapurple">关注</i>
          <i class="el-icon-s-check" style="font-size: 15px;color: rebeccapurple">已被认领</i>
          <i class="el-icon-share" style="font-size: 15px;color: rebeccapurple">分享</i>
          </div>
        </div>

<!--        左2：个人简介-->
        <div class="grid-content bg-purple custom-text" style="margin-top: -400px;">
          <span style="display: inline-block; margin-left: -220px;color: royalblue;font-weight: bold;">· 个人简介</span><br>
          <div style="text-align: center;">
            <hr style="border: none; border-bottom: 2px solid royalblue; width: 280px;">
          </div>
          <div style="text-align: left;padding-left: 10px;font-size: 14px;">
            导学生获得顶尖国际数据挖掘竞赛IJCAI Contest 2015 全球冠军。获得北京市高等学校青年英才和师德先锋等称号。
            CCF-腾讯犀牛鸟基金及项目优秀奖，并指导学生获得顶尖国际数据挖掘竞赛IJCAI Contest 2015 全球冠军。获得北京市高等学校青年英才和师德先锋等称号。
          </div>
        </div>
<!--        左3：教育背景-->
        <div class="grid-content bg-purple custom-text" style="margin-top: -380px;">
          <span style="display: inline-block; margin-left: -220px;color: royalblue;font-weight: bold;">· 教育背景</span><br>
          <div style="text-align: center;">
            <hr style="border: none; border-bottom: 2px solid royalblue; width: 280px;">
          </div>
          <div style="text-align: left;padding-left: 10px;font-size: 14px;">
            导学生获得顶尖国际数据挖掘竞赛IJCAI Contest 2015 全球冠军。获得北京市高等学校青年英才和师德先锋等称号。
          </div>
        </div>
<!--        左4：工作经历-->
        <div class="grid-content bg-purple custom-text" style="margin-top: -380px;">
          <span style="display: inline-block; margin-left: -220px;color: royalblue;font-weight: bold;">· 工作经历</span><br>
          <div style="text-align: center;">
            <hr style="border: none; border-bottom: 2px solid royalblue; width: 280px;">
          </div>
          <div style="text-align: left;padding-left: 10px;font-size: 14px;">
            导学生获得顶尖国际数据挖掘竞赛IJCAI Contest 2015 全球冠军。获得北京市高等学校青年英才和师德先锋等称号。
          </div>
        </div>
<!--        左5-->
        <div class="grid-content bg-purple custom-text" style="margin-top: -300px;">
          <span style="display: inline-block; margin-left: -250px;color: royalblue;font-weight: bold;">· 奖项</span><br>
          <div style="text-align: center;">
            <hr style="border: none; border-bottom: 2px solid royalblue; width: 280px;">
          </div>
          <div style="text-align: left;padding-left: 10px;font-size: 14px;">
            导学生获得顶尖国际数据挖掘竞赛IJCAI Contest 2015 全球冠军。获得北京市高等学校青年英才和师德先锋等称号。
          </div>
        </div>
      </el-aside>


<!--      main-->
      <el-main style="display: flex; flex-direction: column; padding-top: 0;">
        <!-- 中1 -->
        <div style="background-color: white; margin-top: 0; display: flex; flex-direction: column; align-items: flex-start; border: 1px solid lightgray; margin-bottom: 30px;">
          <span style="color: black; font-weight: bold; margin-top: 5px; margin-bottom: 5px; line-height: 1; padding: 10px;">研究兴趣</span>
          <div style="display: flex; justify-content: flex-start;">
            <img class="aminer" src="../assets/img_2.png" style="width: 100%; height: 200px;">
          </div>
        </div>

        <!-- 中2 -->
        <div style="flex: 1; background-color: white; display: flex; flex-direction: column; height: auto; border: 1px solid lightgray;">

<!--            title-->
          <div class="grid-content bg-purple custom-text" style="background-color: pink; display: flex; align-items: center; justify-content: center;border-bottom: 1px solid lightgray;margin-bottom: 0px;">
            <div style="height: 40px;flex-grow: 1; background-color: white; display: flex; align-items: center; justify-content: center;border-right: 1px solid lightgray;">
              <strong>论文</strong>
              <span style="color: dimgray;font-size: 13px;padding-left: 8px;padding-top: 5px">  共399篇</span>
            </div>
            <div style="height: 40px;flex-grow: 1; background-color: white; display: flex; align-items: center; justify-content: center;border-right: 1px solid lightgray;">
              <strong>专利</strong>
              <span style="color: dimgray;font-size: 13px;padding-left: 8px;padding-top: 5px">  共40篇</span>
            </div>
            <div style="height: 40px;flex-grow: 1; background-color: white; display: flex; align-items: center; justify-content: center;border-right: 1px solid lightgray;">
              <strong>基金项目</strong>
              <span style="color: dimgray;font-size: 13px;padding-left: 8px;padding-top: 5px">  共55篇</span>
            </div>
            <div style="height: 40px;flex-grow: 1; background-color: white; display: flex; align-items: center; justify-content: center;"></div>
            <div style="height: 40px;flex-grow: 1; background-color: white; display: flex; align-items: center; justify-content: center;"></div>
          </div>

<!--           paper-info-->
          <div class="grid-content bg-purple custom-text" style="flex: 1; background-color: white; display: flex; flex-direction: column;text-align: left;padding-left: 20px;padding-right: 20px;padding-top: 0px">

            <div  style="flex: 1; background-color: white; border-bottom: 1px dashed lightgray; ">
              <h1 style="font-size: 17px;font-weight:1000;margin-bottom: 3px;"><i class="el-icon-tickets" style="color: red"></i>    Generalizing Graph Neural Networks on Out-of-Distribution Graphs</h1>
              <p style="font-size: 12px;margin-top:0px;margin-bottom: 8px;color: green">Shaohua Fan, Xiao Wang, Chuan Shi, Peng Cui, Bai Wang</p>
              <p style="font-size: 13px;margin-top:0px;margin-bottom: 8px;">IEEE TRANSACTIONS ON PATTERN ANALYSIS AND MACHINE INTELLIGENCE no. 1 (2024): 322-337</p>
<!--              footer-->
              <div style="display: flex;  font-size: 13px;padding-bottom: 10px">
                <div style="width: 10%; border-right: 1px solid lightgray;">
                  <div style="display: flex; align-items: center;">
                    <div style="width: 50%;">引用</div>
                    <div style="width: 50%;color: green">23</div>
                  </div>
                </div>
                <div style="width: 10%; border-right: 1px solid lightgray;">
                  <div style="display: flex; align-items: center;padding-left: 11px;">
                    <div style="width: 50%;">预览</div>
                    <div style="width: 50%;color: green">78</div>
                  </div>
                </div>
                <div style="width: 10%; border-right: 1px solid lightgray;">
                  <div style="display: flex; align-items: center;padding-left: 11px">
                    <i class="el-icon-share" style="color: darkslategrey;padding-right: 4px"></i>
                    <div> 引用</div>
                  </div>
                </div>
              </div>
            </div>

            <div style="flex: 1; background-color: white; border-bottom: 1px dashed lightgray">
              <h1 style="font-size: 17px;font-weight:1000;margin-bottom: 3px;"><i class="el-icon-tickets" style="color: red"></i>    Generalizing Graph Neural Networks on Out-of-Distribution Graphs</h1>
              <p style="font-size: 12px;margin-top:0px;margin-bottom: 8px;color: green">Shaohua Fan, Xiao Wang, Chuan Shi, Peng Cui, Bai Wang</p>
              <p style="font-size: 13px;margin-top:0px;margin-bottom: 8px;">IEEE TRANSACTIONS ON PATTERN ANALYSIS AND MACHINE INTELLIGENCE no. 1 (2024): 322-337</p>

              <div style="display: flex;  font-size: 13px;padding-bottom: 10px">
                <div style="width: 10%; border-right: 1px solid lightgray;">
                  <div style="display: flex; align-items: center;">
                    <div style="width: 50%;">引用</div>
                    <div style="width: 50%;color: green">23</div>
                  </div>
                </div>
                <div style="width: 10%; border-right: 1px solid lightgray;">
                  <div style="display: flex; align-items: center;padding-left: 11px;">
                    <div style="width: 50%;">预览</div>
                    <div style="width: 50%;color: green">78</div>
                  </div>
                </div>
                <div style="width: 10%; border-right: 1px solid lightgray;">
                  <div style="display: flex; align-items: center;padding-left: 11px">
                    <i class="el-icon-share" style="color: darkslategrey;padding-right: 4px"></i>
                    <div> 引用</div>
                  </div>
                </div>
              </div>
            </div>

            <div style="flex: 1; background-color: white; border-bottom: 1px dashed lightgray">
              <h1 style="font-size: 17px;font-weight:1000;margin-bottom: 3px;"><i class="el-icon-tickets" style="color: red"></i>    Generalizing Graph Neural Networks on Out-of-Distribution Graphs</h1>
              <p style="font-size: 12px;margin-top:0px;margin-bottom: 8px;color: green">Shaohua Fan, Xiao Wang, Chuan Shi, Peng Cui, Bai Wang</p>
              <p style="font-size: 13px;margin-top:0px;margin-bottom: 8px;">IEEE TRANSACTIONS ON PATTERN ANALYSIS AND MACHINE INTELLIGENCE no. 1 (2024): 322-337</p>

              <div style="display: flex;  font-size: 13px;padding-bottom: 10px">
                <div style="width: 10%; border-right: 1px solid lightgray;">
                  <div style="display: flex; align-items: center;">
                    <div style="width: 50%;">引用</div>
                    <div style="width: 50%;color: green">23</div>
                  </div>
                </div>
                <div style="width: 10%; border-right: 1px solid lightgray;">
                  <div style="display: flex; align-items: center;padding-left: 11px;">
                    <div style="width: 50%;">预览</div>
                    <div style="width: 50%;color: green">78</div>
                  </div>
                </div>
                <div style="width: 10%; border-right: 1px solid lightgray;">
                  <div style="display: flex; align-items: center;padding-left: 11px">
                    <i class="el-icon-share" style="color: darkslategrey;padding-right: 4px"></i>
                    <div> 引用</div>
                  </div>
                </div>
              </div>
            </div>


            <div style="flex: 1; background-color: white; border-bottom: 1px dashed lightgray">
              <h1 style="font-size: 17px;font-weight:1000;margin-bottom: 3px;"><i class="el-icon-tickets" style="color: red"></i>    Generalizing Graph Neural Networks on Out-of-Distribution Graphs</h1>
              <p style="font-size: 12px;margin-top:0px;margin-bottom: 8px;color: green">Shaohua Fan, Xiao Wang, Chuan Shi, Peng Cui, Bai Wang</p>
              <p style="font-size: 13px;margin-top:0px;margin-bottom: 8px;">IEEE TRANSACTIONS ON PATTERN ANALYSIS AND MACHINE INTELLIGENCE no. 1 (2024): 322-337</p>

              <div style="display: flex;  font-size: 13px;padding-bottom: 10px">
                <div style="width: 10%; border-right: 1px solid lightgray;">
                  <div style="display: flex; align-items: center;">
                    <div style="width: 50%;">引用</div>
                    <div style="width: 50%;color: green">23</div>
                  </div>
                </div>
                <div style="width: 10%; border-right: 1px solid lightgray;">
                  <div style="display: flex; align-items: center;padding-left: 11px;">
                    <div style="width: 50%;">预览</div>
                    <div style="width: 50%;color: green">78</div>
                  </div>
                </div>
                <div style="width: 10%; border-right: 1px solid lightgray;">
                  <div style="display: flex; align-items: center;padding-left: 11px">
                    <i class="el-icon-share" style="color: darkslategrey;padding-right: 4px"></i>
                    <div> 引用</div>
                  </div>
                </div>
              </div>
            </div>


            <div style="flex: 1; background-color: white; border-bottom: 1px dashed lightgray">
              <h1 style="font-size: 17px;font-weight:1000;margin-bottom: 3px;"><i class="el-icon-tickets" style="color: red"></i>    Generalizing Graph Neural Networks on Out-of-Distribution Graphs</h1>
              <p style="font-size: 12px;margin-top:0px;margin-bottom: 8px;color: green">Shaohua Fan, Xiao Wang, Chuan Shi, Peng Cui, Bai Wang</p>
              <p style="font-size: 13px;margin-top:0px;margin-bottom: 8px;">IEEE TRANSACTIONS ON PATTERN ANALYSIS AND MACHINE INTELLIGENCE no. 1 (2024): 322-337</p>

              <div style="display: flex;  font-size: 13px;padding-bottom: 10px">
                <div style="width: 10%; border-right: 1px solid lightgray;">
                  <div style="display: flex; align-items: center;">
                    <div style="width: 50%;">引用</div>
                    <div style="width: 50%;color: green">23</div>
                  </div>
                </div>
                <div style="width: 10%; border-right: 1px solid lightgray;">
                  <div style="display: flex; align-items: center;padding-left: 11px;">
                    <div style="width: 50%;">预览</div>
                    <div style="width: 50%;color: green">78</div>
                  </div>
                </div>
                <div style="width: 10%; border-right: 1px solid lightgray;">
                  <div style="display: flex; align-items: center;padding-left: 11px">
                    <i class="el-icon-share" style="color: darkslategrey;padding-right: 4px"></i>
                    <div> 引用</div>
                  </div>
                </div>
              </div>
            </div>

            <div style="flex: 1; background-color: white; border-bottom: 1px dashed lightgray">块6</div>
            <div style="flex: 1; background-color: white; border-bottom: 1px dashed lightgray">块7</div>
            <div style="flex: 1; background-color: white; border-bottom: 1px dashed lightgray">块8</div>
            <div style="flex: 1; background-color: white; border-bottom: 1px dashed lightgray">块9</div>
            <div style="flex: 1; background-color: white;">块10</div>
          </div>

        </div>

      </el-main>


<!--          右侧-->

      <el-aside style="width:320px; display: flex; flex-direction: column; padding-top: 0;margin-right: 10px;">
        <div style="background-color: white; margin-top: 0; display: flex; flex-direction: column; align-items: flex-start; border: 1px solid lightgray; margin-bottom: 30px;">
          <span style="color: black; font-weight: bold; margin-top: 5px; margin-bottom: 5px; line-height: 1; padding: 10px;">作者统计</span>
          <div style="display: flex; justify-content: flex-start;padding-bottom: 18px;padding-top: 10px;flex-direction:row;">
            <div><img class="aminer" src="../assets/img_3.png" style=" height: 170px;"></div>
            <div v-for="book in books" :key="book.id" style="display: flex; flex-direction: column; justify-content: center; ">
              <div style="margin: -80px 0;">#npubs: {{ book.npubs }}</div>
              <div style="margin: -80px 0;">#ncitation: {{ book.ncitation }}</div>
              <div style="margin: -80px 0;">#hindex: {{ book.hindex }}</div>
            </div>

          </div>
        </div>

        <div style="flex: 1; background-color: white; display: flex; flex-direction: column; height: auto; border: 1px solid lightgray;margin-top: 0px">
<!--          title-->
          <div class="grid-content bg-purple custom-text" style="background-color: pink; display: flex; justify-content: center; align-items: center;border-bottom: 1px solid lightgray;margin-bottom: 0px;">
            <div style="height: 40px; flex-grow: 1; background-color: white; display: flex; align-items: center; justify-content: center;">合作学者</div>
            <div style="height: 40px;flex-grow: 1; background-color: white; display: flex; align-items: center; justify-content: center;">合作机构</div>
            <div style="height: 40px;flex-grow: 1; background-color: white; display: flex; align-items: center; justify-content: center;">D-Core</div>
            <div style="height: 40px;flex-grow: 1; background-color: white; display: flex; align-items: center; justify-content: center;">
              <i class="el-icon-s-data" style="height: 60px;width:50px;padding-top: 40px;color: blue"></i>
            </div>
            <div style="height: 40px;flex-grow: 1; background-color: white; display: flex; align-items: center; justify-content: center;"></div>
          </div>
<!--              autor-info-->
          <div class="grid-content bg-purple custom-text" style="flex: 1; background-color: white; display: flex; flex-direction: column;text-align: left;padding-top: -20px">

<!--            autor-1-->
            <div style="display: flex; justify-content: space-between; background-color: white; border-bottom: 1px dashed lightgray;">
              <div style="width: 25%;background-color: white;display: flex; justify-content: center; align-items: center;">
                <img class="aminer" src="../assets/img_1.png" style=" width:50px;height:50px; border: 1px solid green;border-radius: 50%;">
              </div>
              <div style="width: 50%;">
                <h1 style="font-size: 14px; font-weight: 1000; margin-bottom: 3px;">Xiao Wang (王啸）</h1>
                <p style="font-size: 13px; margin-top: 0; margin-bottom: 8px; color: dimgray;">School of Software</p>
              </div>
              <div style="flex: 1;background-color: white;text-align: center;">
                <h1 style="font-size: 12px;  margin-bottom: 3px;color: dimgray">合作论文数</h1>
                <p style="font-size: 16px; margin-top: 0; margin-bottom: 8px; color: blue;">30</p>
              </div>
            </div>

<!--            autor-2-->
            <div style="display: flex; justify-content: space-between; background-color: white; border-bottom: 1px dashed lightgray;">
              <div style="width: 25%;background-color: white;display: flex; justify-content: center; align-items: center;">
                <img class="aminer" src="../assets/img_1.png" style=" width:50px;height:50px; border: 1px solid green;border-radius: 50%;">
              </div>
              <div style="width: 50%;">
                <h1 style="font-size: 14px; font-weight: 1000; margin-bottom: 3px;">Xiao Wang (王啸）</h1>
                <p style="font-size: 13px; margin-top: 0; margin-bottom: 8px; color: dimgray;">School of Software</p>
              </div>
              <div style="flex: 1;background-color: white;text-align: center;">
                <h1 style="font-size: 12px;  margin-bottom: 3px;color: dimgray">合作论文数</h1>
                <p style="font-size: 16px; margin-top: 0; margin-bottom: 8px; color: blue;">30</p>
              </div>
            </div>

<!--              autor-3-->

            <div style="display: flex; justify-content: space-between; background-color: white; border-bottom: 1px dashed lightgray;">
              <div style="width: 25%;background-color: white;display: flex; justify-content: center; align-items: center;">
                <img class="aminer" src="../assets/img_1.png" style=" width:50px;height:50px; border: 1px solid green;border-radius: 50%;">
              </div>
              <div style="width: 50%;">
                <h1 style="font-size: 14px; font-weight: 1000; margin-bottom: 3px;">Xiao Wang (王啸）</h1>
                <p style="font-size: 13px; margin-top: 0; margin-bottom: 8px; color: dimgray;">School of Software</p>
              </div>
              <div style="flex: 1;background-color: white;text-align: center;">
                <h1 style="font-size: 12px;  margin-bottom: 3px;color: dimgray">合作论文数</h1>
                <p style="font-size: 16px; margin-top: 0; margin-bottom: 8px; color: blue;">30</p>
              </div>
            </div>

          </div>
        </div>

      </el-aside>


    </el-container>
  </el-container>

</template>

<style scoped>
.fixed-header {
  position: fixed;
  top: 0;
  width: 100%;
  z-index: 999; /* 确保 Header 处于最顶层 */

}

.main-container {
  padding-top: 60px; /* 调整容器上边距，避免内容被 Header 遮挡 */
}
.el-header, .el-footer {

  background: linear-gradient(to right, midnightblue, purple);
  color: #333;
  text-align: center;
  line-height: 60px;
}

.el-aside {
  background-color: white;
  color: #333;
  text-align: center;
  line-height: 200px;
}

.el-main {
  background-color: white;
  color: #333;
  text-align: center;
  line-height: 160px;
}

body > .el-container {
  margin-bottom: 40px;
}

.el-container:nth-child(5) .el-aside,
.el-container:nth-child(6) .el-aside {
  line-height: 260px;
}

.el-container:nth-child(7) .el-aside {
  line-height: 320px;
}
.aminer{
  width: 180px;
  height: 60px;
}

.flex-container {
  display: flex;
  flex-direction: column; /* 垂直布局 */
  justify-content: space-between; /* 均匀分配子项，上下留有空间 */
}

.custom-text {
  line-height: 1.5; /* 设置合适的行高 */
  margin-bottom: 30px;
}

</style>
