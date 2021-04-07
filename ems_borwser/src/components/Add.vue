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
                    添加员工:
                </h1>
                <table cellpadding="0" cellspacing="0" border="0"
                       class="form_table">
                    <tr>
                        <td valign="middle" align="right">名称:</td>
                        <td valign="middle" align="left">
                            <input type="text" class="inputgri" name="name" v-model="name"/>
                        </td>
                    </tr>
                    <tr>
                        <td valign="middle" align="right">图片:</td>
                        <td valign="middle" align="left">
                            <input type="file" name="photo" ref="photo"/>
                        </td>
                    </tr>
                    <tr>
                        <td valign="middle" align="right">工资:</td>
                        <td valign="middle" align="left">
                            <input type="text" class="inputgri" v-model="salary"/>
                        </td>
                    </tr>
                    <tr>
                        <td valign="middle" align="right">年龄:</td>
                        <td valign="middle" align="left">
                            <input type="text" class="inputgri" v-model="age"/>
                        </td>

                    </tr>

<!--                    <tr>-->
<!--                    <td valign="middle" align="right">部门:</td>-->
<!--                        <td valign="middle" align="left">-->
<!--                            <select name="dept" id="" v-model="dept">-->
<!--                                    <option v-for="(dept,i) in deptlist":key="i" :value="dept.id">{{ dept.dept_name }}</option>-->
<!--                            </select>-->
<!--                        </td>-->
<!--                    </tr>-->

                </table>
                <p>
                    <input type="submit" class="button" value="提交" @click="add_emp"/>
                </p>
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
    name: "Add",
    data() {
        return {
            age: "",
            salary: "",
            name: "",
        }
    },

    methods: {
        // 添加员工  向后台发送请求完成员工添加
        add_emp() {
            // 获取图片的方式  this.$refs
            let photo = this.$refs.photo.files[0];
            // ajax上传文件必须要用formData对象
            let formData = new FormData();

            // 将所有的属性添加至formData中
            formData.append("emp_name", this.name)
            formData.append("age", this.age)
            formData.append("salary", this.salary)
            formData.append("photo", this.$refs.photo.files[0]);

            this.$axios({
                url: "http://127.0.0.1:8000/emp/employees/",
                method: 'post',
                data: formData,
                // 指定请求头相关参数
                headers: {
                    // 当前请求时包含文件
                    'content-type': "multipart/form-data"
                },
            }).then(res => {
                console.log(res.data);
                this.$message.success("员工添加成功");
                // 添加成功后跳转至列表页
                this.$router.push("/index");
            }).catch(error => {
                console.log(error);
                this.$message.error("添加失败");
            })
        },
    }
}
</script>

<style scoped>

</style>
