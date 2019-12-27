<template id="courseType">
    <el-row style="height: 100%;border: 1px solid #DCDFE6;margin-top: 10px">
        <el-col :span="6" style="border-right: 1px solid #DCDFE6; min-height:500px;">
            <div class="grid-content bg-purple">
                <el-tree  style="border: none" :data="courseTypes" :props="defaultProps" accordion  @node-click="handleNodeClick">

                </el-tree>
            </div>
        </el-col>

        <el-col :span="17" style="margin-left: 10px;padding-top: 10px">
            <div class="grid-content bg-purple">
                <template>
                    <el-table
                            :data="courseTypes1"
                            style="width: 100%">
                            <el-table-column type="index" width="40" label="#">
                            </el-table-column>

                            <el-table-column prop="createTime" label="创建日期" :formatter="dateFormat1" width="130" sortable>
                            </el-table-column>

                            <el-table-column property="createTime"  label="创建时间" width="120" sortable>
                                <template scope="scope">
                                    {{ dateTimeFormat(scope.row.createTime) }}
                                </template>
                            </el-table-column>

                            <el-table-column prop="类型名" label="name" width="100" sortable>
                            </el-table-column>

                            <el-table-column prop="pid" label="父级" width="110" sortable>
                            </el-table-column>

                            <el-table-column prop="logo" label="LOGO" width="120"  sortable>
                            </el-table-column>

                            <el-table-column prop="sortIndex" label="sortIndex" width="110" sortable>
                            </el-table-column>

                            <el-table-column prop="description" label="备注" width="90" sortable>
                            </el-table-column>

                            <el-table-column prop="path" label="路径?" width="90" sortable>
                            </el-table-column>

                        <el-table-column label="操作">
                            <template slot-scope="scope">
                                <el-button
                                        size="mini"
                                        round plain
                                        @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                                <el-button
                                        size="mini"
                                        type="danger"
                                        round
                                        plain
                                        @click="handleDelete(scope.$index, scope.row)">删除</el-button>
                            </template>
                        </el-table-column>
                    </el-table>
                </template>
            </div>

        </el-col>
    </el-row>
</template>
<style>
    .el-row {
        margin-bottom: 20px;
        height: 100%;
    }
    :last-child {
        margin-bottom: 0;
    }
    #courseType el-col {
        border: 1px solid red;
        border-radius: 4px;
    }
    .grid-content {
        border-radius: 4px;
        min-height: 36px;
    }
</style>

<script>
    export default {
        data() {
            return {
                course:[],
                courseTypes:[],
                courseTypes1:[],
                defaultProps: {
                    children: 'children',
                    label: 'name'
                }
            }
        },
        methods:{
            getTreeData(){
                // 发送一个异步请求: get请求 /product/courseType/treeData
                this.$http.post("/courseType/courseType/treeData").then(res=>{
                    console.log(this);
                    this.courseTypes = res.data;
                    this.courseTypes1 = res.data;
                });
            },
            getCourse(){
                this.$http.post("/courseType/courseType/page").then(res=>{
                    console.log(this);
                    this.course = res.data;
                });
            },
            handleNodeClick(val){
                this.courseTypes1 = val.children;
            },
            //修改日期格式化
            dateTimeFormat(value) {
                let time = new Date(+value);
                let rightTwo = (v) => {
                    v = '0' + v
                    return v.substring(v.length - 2, v.length)
                }
                if (time == null) return;
                let year = time.getFullYear();
                let month = time.getMonth() + 1;
                let date = time.getDate();
                let hours = time.getHours();
                let minutes = time.getMinutes();
                let seconds = time.getSeconds();
                return year + '-' + rightTwo(month) + '-' + rightTwo(date)+ ' ' + rightTwo(hours) + ':' + rightTwo(minutes) + ':' + rightTwo(seconds);
            },
            //修改日期
            dateFormat2(row){
                var t=new Date(row.updateTime);//row 表示一行数据, updateTime 表示要格式化的字段名称
                return t.getFullYear()+"-"+(t.getMonth()+1)+"-"+t.getDate();//+" "+t.getHours()+":"+t.getMinutes()+":"+t.getSeconds()+"."+t.getMilliseconds()
            }
        },


        mounted(){
            //对courseTypes数据赋值
           this.getTreeData();
        }
    };
</script>