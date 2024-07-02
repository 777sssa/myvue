<template>
    <el-header class="fixed-header" style="margin-bottom: 0px;">
        <el-row>
            <hr>
            <el-col :span="4">
                <div class="grid-content bg-purple"><img class="aminer" src="../assets/img.png"></div>
            </el-col>
            <el-col :span="2">
                <div class="grid-content bg-purple">333</div>
            </el-col>
            <el-col :span="2">
                <div class="grid-content bg-purple">333</div>
            </el-col>
            <el-col :span="8">
                <div class="grid-content bg-purple-light">
                    <input
                            v-model="searchQuery"
                            placeholder="搜索作者名称"
                            @input="searchBooks"
                            style="width: 80%; height: 20px; padding: 10px; font-size: 16px; border-radius: 5px; border: 1px solid #ccc; margin-bottom: 10px;"
                    />
                </div>
            </el-col>
            <el-col :span="4">
                <div class="grid-content bg-purple">333</div>
            </el-col>

            <el-col :span="1">
                <div class="grid-content bg-purple">
                    <i class="el-icon-upload" style="color: white;font-size: 30px"></i>
                </div>
            </el-col>
            <el-col :span="1">
                <div class="grid-content bg-purple">
                    <i class="el-icon-message-solid" style="color: white;font-size: 30px"></i>
                </div>
            </el-col>
            <el-col :span="1">
                <div class="grid-content bg-purple">
                    <i class="el-icon-s-tools " style="color: white;font-size: 30px"></i>
                </div>
            </el-col>
            <el-col :span="1">
                <div class="grid-content bg-purple" style="color: white;">未登录</div>
            </el-col>

            <hr>

        </el-row>
    </el-header>

</template>

<script>
    export default {
        name: "Header"
    }
</script>

<style scoped>

</style>