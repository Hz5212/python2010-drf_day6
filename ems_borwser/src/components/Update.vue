<template>
    <div id="wrap">
        <div id="top_content">
            <div id="header">
                <div id="rightheader">
                    <p>
                        2009/11/20
                        <br/>
                    </p>
                </div>
                <div id="topheader">
                    <h1 id="title">
                        <a href="#">Main</a>
                    </h1>
                </div>
                <div id="navigation">
                </div>
            </div>
            <div id="content">
                <p id="whereami">
                </p>
                <h1>
                    更新员工:
                </h1>
                <form>
                    <table cellpadding="0" cellspacing="0" border="0"
                           class="form_table">
<!--                        <tr>-->
<!--                            <td valign="middle" align="right">id:</td>-->
<!--                            <td valign="middle" align="left">-->
<!--                                {{ id }}-->
<!--                            </td>-->
<!--                        </tr>-->
                        <tr>
                            <td valign="middle" align="right">姓名:</td>
                            <td valign="middle" align="left">
                                <input type="text" class="inputgri" name="name" v-model="name"/>
                            </td>
                        </tr>
                        <tr>
                            <td valign="middle" align="right">照片:</td>
                            <td valign="middle" align="left">
                                <input type="file" name="photo" ref="photo"/>
                            </td>
                        </tr>
                        <tr>
                            <td valign="middle" align="right">工资:</td>
                            <td valign="middle" align="left">
                                <input type="text" class="inputgri" name="salary" v-model="salary"/>
                            </td>
                        </tr>
                        <tr>
                            <td valign="middle" align="right">年龄:</td>
                            <td valign="middle" align="left">
                                <input type="text" class="inputgri" name="age" v-model="age"/>
                            </td>

                        </tr>

<!--                        <tr>-->
<!--                        <td valign="middle" align="right">部门:</td>-->
<!--                        <td valign="middle" align="left">-->
<!--                            <select name="dept" id="" v-model="dept">-->
<!--                                    <option v-for="(dept,i) in deptlist":key="i" :value="dept.id">{{ dept.dept_name }}</option>-->
<!--                            </select>-->
<!--                        </td>-->
<!--                        </tr>-->

                    </table>
                    <p>
                        <input type="submit" class="button" value="提交" v-on:click="updata"/>
                    </p>
                </form>
            </div>
        </div>
        <div id="footer">
            <div id="footer_bg">
                ABC@126.com
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "Update",
    data() {
        return {
            age: "",
            salary: "",
            name: "",
            deptlist:[],
        }
    },
    methods: {
        updata() {
            // 获取id的方式
            let emp_id = this.$route.params.id;
            // ajax上传文件必须要用formData对象
            let formData = new FormData();

            // 将所有的属性添加至formData中
            formData.append("emp_name", this.name)
            formData.append("age", this.age)
            formData.append("salary", this.salary)
            formData.append("photo", this.$refs.photo.files[0]);

            this.$axios({
                //将id拼接到url中
                url: "http://127.0.0.1:8000/emp/employees/" + emp_id + "/",
                method: 'patch',
                data: formData,
                // 指定请求头相关参数
                headers: {
                    // 当前请求时包含文件
                    'content-type': "multipart/form-data"
                },
            }).then(res => {
                console.log(res.data);
                this.$message.success("员工修改成功");
                // 添加成功后跳转至列表页
                this.$router.push("/index");
            }).catch(error => {
                console.log(error);
                this.$message.error("修改失败");
                this.$router.push("/index");
            })
        },
        get_dept_list(){
                this.$axios({
                    url: "http://127.0.0.1:8000/emp/dept/",
                    method: "get",
                }).then(res => {
                    this.deptlist = res.data
                }).catch(error => {
                    console.log(error);
                })
            },
    },
    created() {
            let id = sessionStorage.id;
            if (id) {
                this.$message.success("正在跳转");
            } else {
                this.$message.error("对不起，您还未登录，请登录后在访问");
                this.$router.push("/login");
            }
            let emp_id = this.$route.params.id
            this.$axios({
                url: "http://127.0.0.1:8000/emp/employees/" + emp_id + "/",
                method: 'get'
            }).then(res => {
                console.log(res.data)
                this.name = res.data.emp_name
                this.photo = res.data.photo
                this.age = res.data.age
                this.salary = res.data.salary
                console.log(res.data)
            }).catch(error => {
                console.log(error);
                this.$message.error("tiaozhuan失败")
            })
            // 调用获取员工列表数据的方法
            this.get_dept_list()

        },
}
</script>

<style scoped>

</style>
