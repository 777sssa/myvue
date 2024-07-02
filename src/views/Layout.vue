<script>
    import Aside from '../components/Aside.vue'
    import axios from 'axios';
    import {ElPagination, Pagination} from 'element-ui';
    import {Chart, registerables} from 'chart.js';
    import * as d3 from 'd3';
    import d3Cloud from 'd3-cloud';

    Chart.register(...registerables);

    export default {
        name: "Layout",
        components: {
            Aside,
            ElPagination:Pagination
        },
        data() {
            return {
                searchQuery: '',
                books: [],
                papers: [],
                currentPage: 2,
                pageSize: 15,
                totalPapers: 1,
                displayedPapers: [],
                chartData: {
                    labels: ['hindex', '出版物数量', '引用量'],
                    datasets: [{
                        data: [65, 78, 12],
                        backgroundColor: [
                            'rgba(255, 99, 132, 0.5)',  // 引用的颜色
                            'rgba(54, 162, 235, 0.5)',  // H-Index的颜色
                            'rgba(75, 192, 192, 0.5)'   // G-Index的颜色
                        ],
                        borderColor: [
                            'rgb(255, 99, 132)',  // 引用的边框颜色
                            'rgb(54, 162, 235)',  // H-Index的边框颜色
                            'rgb(75, 192, 192)'   // G-Index的边框颜色
                        ],
                        borderWidth: 1  // 所有扇形区域的边框宽度
                    }]
                },
            };
        },
        methods: {
            async searchBooks() {
                try {
                    const response = await axios.get(`http://localhost:8180/book/findByName`, {
                        params: {
                            name: this.searchQuery,
                            page: this.currentPage,
                            size: this.pageSize
                        }
                    });
                    this.books = response.data.author;
                    this.papers = response.data.papers;

                    this.totalPapers = this.papers.length; // 总论文数量
                    this.updateDisplayedPapers(); // 更新显示的论文

                    // this.chartData.datasets[0].data[0] = this.books
                    this.chartData.datasets[0].data[0] = this.books.hindex;
                    this.chartData.datasets[0].data[1] = this.books.npubs;
                    this.chartData.datasets[0].data[2] = this.books.ncitation;

                    console.log("**********")
                    console.log(this.papers)
                    console.log("**********")

                    this.$nextTick(() => {
                        this.renderChart();
                    });

                } catch (error) {
                    console.error('数据出错:', error);
                }
            },
            renderChart() {
                if (this.chart) {
                    this.chart.destroy(); // 销毁旧的图表实例
                }
                this.chart = new Chart(
                    this.$refs.radarChart, // 使用 ref 引用
                    {
                        type: 'polarArea',
                        data: this.chartData
                    }
                );
            },
            handlePageChange(page) {
                this.currentPage = page;
                // this.searchBooks();
                this.updateDisplayedPapers();
                console.log('Current Page:', this.currentPage);
                console.log('Displayed Papers:', this.displayedPapers);

            },
            updateDisplayedPapers() {
                const start = (this.currentPage - 1) * this.pageSize;
                const end = start + this.pageSize;
                this.displayedPapers = this.papers.slice(start, end);
            }
        },
        mounted() {
            this.searchBooks(); // 页面加载时执行一次搜索
            // new Chart(
            //     document.getElementById('radarChart'),
            //     {
            //         type: 'polarArea',
            //         data: this.chartData
            //     }
            // );
        }
    };

</script>

<template>

    <!--  header-->

    <el-container class="main-container">
        <div style="width: 100%;">

            <el-header class="fixed-header" style="margin-bottom: 0px; ">
                <!-- 这里是你想要添加的 div 块的内容 -->
                <el-row type="flex" justify="center">
                    <el-col :span="4">
                        <div class="grid-content logo" style="margin-bottom: 20px;">
                            <!--            <img src="../assets/img_4.png" alt="Logo" class="logo-image" style="width: 200px;">-->
                        </div>
                    </el-col>
                    <el-col :span="16">
                        <div class="grid-content menu">
                            <el-menu mode="horizontal" class="el-menu-demo" background-color="#fff" text-color="#333"
                                     active-text-color="#409EFF">
                                <el-menu-item index="1">研创中心</el-menu-item>
                                <el-menu-item index="2">知识服务</el-menu-item>
                                <el-menu-item index="3">订阅中心</el-menu-item>
                                <el-menu-item index="4">期刊大全</el-menu-item>
                                <el-menu-item index="5">报刊大全</el-menu-item>
                            </el-menu>
                        </div>
                    </el-col>
                    <el-col :span="4">
                        <div class="grid-content user-menu"
                             style="display: flex; justify-content: center; align-items: center; height: 100%">
                            <el-button type="primary" style="margin-right: 10px;">登录</el-button>
                            <el-button>注册</el-button>
                        </div>
                    </el-col>
                </el-row>
            </el-header>
            <!--    <el-header class="fixed-header" style="margin-bottom: 0px;">-->
            <div class="top-div">
                <el-row type="flex" justify="center">
                    <el-col :span="6">
                        <div class="grid-content bg-purple"></div>
                    </el-col>
                    <el-col :span="12">
                        <div class="grid-content bg-purple-light"
                             style="display: flex; justify-content: center; align-items: center;">
                            <input
                                    v-model="searchQuery"
                                    placeholder="搜索作者名称"
                                    @input="searchBooks"
                                    style="width: 100%; height: 20px; padding: 10px; font-size: 16px; border-radius: 5px; border: 1px solid #ccc;margin-top: 30px;"
                            />
                        </div>
                    </el-col>
                    <el-col :span="6">
                        <div class="grid-content bg-purple"
                             style="display: flex; justify-content: flex-end; align-items: center;">
                            <i class="el-icon-upload" style="color: white;font-size: 20px; margin-right: 20px;"></i>
                            <i class="el-icon-message-solid"
                               style="color: white;font-size: 20px; margin-right: 20px;"></i>
                            <i class="el-icon-s-tools" style="color: white;font-size: 20px; margin-right: 20px;"></i>
                            <span style="color: white;margin-right: 20px;">未登录</span>
                        </div>
                    </el-col>
                </el-row>
            </div>

        </div>


        <div class="main-body" style="width: 2000px; height: 1000px; border: 2px solid red">
            <!--      left zuozhexinxi lunwen-->
            <div class="main-body-left" style="width: 80%; height: 100%; border: 2px solid black">
                <!--        zuozhexinxi-->
                <div class="profile-card">

                    <div>
                        <img v-if="books.picture" :src="books.picture" alt="Profile Picture" class="profile-pic">
                    </div>

                    <div class="profile-info">
                        <h1>{{ books.authorName }}</h1>
                        <p v-if="books.org">{{ books.org }}</p>
                        <p v-if="books.pos">{{ books.pos }}</p>
                        <p v-if="books.email">📧 {{ books.email }}</p>
                        <p v-if="books.phone">📞 {{ books.phone }}</p>
                        <p v-if="books.gender">🚹 {{ books.gender }}</p>
                        <p v-if="books.education">{{ books.education }}</p>
                        <a v-if="books.homepage" :href="books.homepage" target="_blank">Homepage</a>
<!--                        <div v-if="books.interests && books.interests.length">-->
<!--                            <span v-for="interest in books.interests" :key="interest" class="interest">{{ interest }}-->
<!--                            <template>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</template></span>-->
<!--                        </div>-->
<!--                        <div v-if="books.interests && books.interests.length" class="interests-container">-->
<!--                            <span v-for="(interest, index) in books.interests" :key="index" class="interest">-->
<!--                              {{ interest }}<template v-if="index < interests.length - 1">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</template>-->
<!--                            </span>-->
<!--                        </div>-->
                    </div>

                </div>
                <!--        lunwen-->
                <div class="lunwen" style="width: 80%; height: 100%;">

                    <div class="content-wrapper">
                        <div class="grid-content bg-purple custom-text" style="background-color: pink; display: flex; align-items: center; justify-content: center;border-bottom: 1px solid lightgray;margin-bottom: 0px;">
                            <div style="height: 40px;flex-grow: 1; background-color: white; display: flex; align-items: center; justify-content: center;border-right: 1px solid lightgray;">
                                <strong>论文</strong>
                                <span style="color: dimgray;font-size: 13px;padding-left: 8px;padding-top: 5px"> </span>
                            </div>
                        </div>

                        <div v-if="displayedPapers.length" class="bg-purple custom-text" style="flex: 1; background-color: white; display: flex; flex-direction: column; text-align: left; padding-left: 20px; padding-right: 20px; padding-top: 0px">
                            <div v-for="paper in displayedPapers" :key="paper.id" style="flex: 1; background-color: white; border-bottom: 1px dashed lightgray;">
                                <h1 style="font-size: 17px; font-weight: 1000;"><i class="el-icon-tickets" style="color: red"></i>
                                    {{ paper.title }}
                                </h1>
                                <p style="font-size: 12px; color: green">
                                    {{ paper.abstract }}
                                </p>
                                <p style="font-size: 13px;">
                                    {{paper.venue}}
                                </p>
                                <p style="font-size: 13px;">
                                    引用量：{{paper.ncitation}}
                                    发表年份：{{paper.year}}
                                </p>
                            </div>
                        </div>

<!--                        <div v-if="displayedPapers.length" class="grid-content bg-purple custom-text">-->
<!--                            <div v-for="paper in displayedPapers" :key="paper.id" class="paper-item">-->
<!--                                <h1 class="paper-title"><i class="el-icon-tickets" style="color: red"></i> {{ paper.title }}</h1>-->
<!--                                <p class="paper-abstract">{{ paper.abstract }}</p>-->
<!--                                <p class="paper-details">{{paper.venue}}</p>-->
<!--                                <p class="paper-details">引用量：{{paper.nCitation}} 发表年份：{{paper.year}}</p>-->
<!--                            </div>-->
<!--                        </div>-->
                        <el-pagination
                                style="margin-top: 20px; text-align: center;"
                                background
                                layout="prev, pager, next"
                                :current-page="currentPage"
                                :page-size="pageSize"
                                :total="totalPapers"
                                @current-change="handlePageChange"
                        ></el-pagination>
                    </div>

                </div>

            </div>

            <!--      right keshihua-->
            <div style="width: 20%; height: 100%; border: 2px solid black">

                <div>

                    {{books.tags}}
                </div>

                <div style="background-color: white; margin-top: 0; display: flex; flex-direction: column; align-items: flex-start; border: 1px solid lightgray; margin-bottom: 30px;">
                    <span style="color: black; font-weight: bold; margin-top: 5px; margin-bottom: 5px; line-height: 1; padding: 10px;">作者统计</span>
                    <div style="display: flex; flex-direction: row; align-items: flex-start; justify-content: space-between; align-items: flex-start; position: relative">
<!--                        <div style="width: 80%">-->
                        <div>
                            <canvas ref="radarChart"></canvas>
                        </div>
                        <div style="display: block; unicode-bidi: isolate;">
                            <p v-for="(value, index) in chartData.datasets[0].data" :key="index"
                               style="font-size: 10px; line-height: 20px; margin: 0 0;">
                                <span><strong>{{ chartData.labels[index] }}:</strong></span>
                                <span>{{ value }}</span>
                            </p>
                        </div>
                    </div>
                </div>

            </div>
        </div>
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
        padding-top: 0px; /* 调整容器上边距，避免内容被 Header 遮挡 */
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }

    .main-body {
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: center;
    }

    .main-body-left {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }

    .el-header, .el-footer {

        background: white;
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

    .profile-card {
        display: flex;
        flex-direction: row;
        align-items: center;
        border: 2px solid #000;
        padding: 20px;
        margin: 30px auto;
        /*max-width: 600px;*/
        width: 80%;
    }

    .profile-pic {
        width: 100px;
        height: 100px;
        border-radius: 50%;
        margin-right: 20px;
    }

    .profile-info {
        flex: 1;
    }

    .profile-info h1 {
        margin: 0;
        font-size: 24px;
    }

    .profile-info p, .profile-info a {
        margin: 4px 0;
        font-size: 16px;
        color: #333;
    }

    .interest {
        display: inline-block;
        background: #eee;
        padding: 5px 10px;
        margin: 5px;
        border-radius: 10px;
        font-size: 14px;
    }

    .aminer {
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

    /* 分页距离右边20，距离底边20 */
    .pagination-container .el-pagination {
        position: absolute;
        right: 20px;
        bottom: 20px;

    }

    .top-div {
        background-color: royalblue;
        /*padding:  0;*/
        width: 100%;
        /*margin-top: 30px;*/
    }

    .grid-content {
        text-align: center;
        line-height: 120px;
    }

    .content-wrapper {
        width: 80%;
        margin: 0 auto;
        background-color: white;
        padding: 20px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    }

    .lunwen {
        height: 100%;
        border: 2px solid pink;
        display: flex;
        flex-direction: column;
        overflow-y: auto; /* 使论文部分可以滚动 */
    }

</style>
