<template>
    <div>
        <el-row :gutter="20">
            <el-col>
                <el-row>
                    <el-col>
                        <el-card shadow="hover" class="mgb20">
                            <div class="user-info">
                                <img src="static/img/img.jpg" class="user-avator" alt="">
                                <div class="user-info-cont">
                                    <div class="user-info-name">{{name}}</div>
                                    <div>姓名：{{userData.name}}</div>
                                    <div>项目管理员id：{{userData.id}}</div>
                                    <div>性别：{{userData.gender}}</div>
                                    <div>地址：{{userData.address}}</div>
                                </div>
                            </div>
                        </el-card>

                    </el-col>
                </el-row>
            </el-col>

        </el-row>
    </div>
</template>

<script>
import { getStore,setStore } from "@/config/mUtils";
import api from "@/apis";
export default {
    name: "dashboard",
    data() {
        return {
            name: getStore("ms_username"),
            userData: {}
        };
    },
    created() {
        this.getData();
    },
    methods: {
        async getData() {
            const res = await api.getUser({
                token: getStore("token")
            });
            if (res.data.code == "0") {
                console.log(res.data);
                if (res.data.isSystemAdmin == "1") {
                    this.isSystemAdmin = "超级管理员";
                    setStore('isSystemAdmin',res.data.isSystemAdmin);
                } else {
                    this.isSystemAdmin = "普通用户";
                }
                if (res.data.data.gender == "0") {
                    res.data.data.gender = "女";
                }
                if (res.data.data.gender == "1") {
                    res.data.data.gender = "男";
                }
                if (res.data.data.gender == "2") {
                    res.data.data.gender = "保密";
                }
                this.userData = res.data.data;
            }
        }
    }
};
</script>


<style scoped>
.el-row {
    margin-bottom: 20px;
}

.grid-content {
    display: flex;
    align-items: center;
    height: 100px;
}

.grid-cont-right {
    flex: 1;
    text-align: center;
    font-size: 12px;
    color: #999;
}

.grid-num {
    font-size: 30px;
    font-weight: bold;
}

.grid-con-icon {
    font-size: 50px;
    width: 100px;
    height: 100px;
    text-align: center;
    line-height: 100px;
    color: #fff;
}

.grid-con-1 .grid-con-icon {
    background: rgb(45, 140, 240);
}

.grid-con-1 .grid-num {
    color: rgb(45, 140, 240);
}

.grid-con-2 .grid-con-icon {
    background: rgb(100, 213, 114);
}

.grid-con-2 .grid-num {
    color: rgb(45, 140, 240);
}

.grid-con-3 .grid-con-icon {
    background: rgb(242, 94, 67);
}

.grid-con-3 .grid-num {
    color: rgb(242, 94, 67);
}

.user-info {
    display: flex;
    align-items: center;
    padding-bottom: 20px;
    border-bottom: 2px solid #ccc;
    margin-bottom: 20px;
}

.user-avator {
    width: 120px;
    height: 120px;
    border-radius: 50%;
}

.user-info-cont {
    padding-left: 50px;
    flex: 1;
    font-size: 14px;
    color: #999;
}

.user-info-cont div:first-child {
    font-size: 30px;
    color: #222;
}

.user-info-list {
    font-size: 14px;
    color: #999;
    line-height: 25px;
}

.user-info-list span {
    margin-left: 70px;
}

.mgb20 {
    margin-bottom: 20px;
}

.todo-item {
    font-size: 14px;
}

.todo-item-del {
    text-decoration: line-through;
    color: #999;
}
</style>
