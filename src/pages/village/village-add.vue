<template>
    <div class="add-village">
        <div class="header">
            <div class="nav-bar">
                <p class="font16">添加村庄</p>
                <router-link to="/village">
                    <el-button class="back-btn">返回</el-button>
                </router-link>
            </div>
        </div>
        <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm" style="width:600px">
            <el-form-item label="村庄名称" prop="name">
                <el-input v-model="ruleForm.name"></el-input>
            </el-form-item>
            <el-form-item label="地理位置" prop="address">
                <el-cascader
                    :options="options"
                    @active-item-change="handleItemChange"
                    :props="props"
                ></el-cascader>
            </el-form-item>
            <el-form-item label="常住人口" prop="people">
                <el-input v-model="ruleForm.people"></el-input>
            </el-form-item>
            <el-form-item label="文字简介" prop="desc">
                <el-input type="textarea" v-model="ruleForm.desc"></el-input>
            </el-form-item>
            <el-form-item label="上传图片" prop="upload">
                <el-upload
                  action="https://jsonplaceholder.typicode.com/posts/"
                  list-type="picture-card"
                  :multiple="true"
                  :on-success="handleUploadSuccess"
                  :on-preview="handlePictureCardPreview"
                  :on-remove="handleRemove">
                  <i class="el-icon-plus"></i>
                </el-upload>
                <el-dialog v-model="dialogVisible" size="tiny">
                  <img width="100%" :src="dialogImageUrl" alt="">
                </el-dialog>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="submitForm('ruleForm')">提交</el-button>
                <el-button @click="resetForm('ruleForm')">重置</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>

<script>
    import axios from 'axios';
    export default {
        data() {
            return {
                ruleForm: {
                    name: '',
                    address: '',
                    people: '',
                    desc: '',
                    picture: [],
                },
                
                selectedOptions: '',
                
                options: [{
                    label: '江苏',
                    cities: []
                }, {
                    label: '浙江',
                    cities: []
                }],
                
                props: {
                    value: 'label',
                    children: 'cities'
                },
                
                dialogImageUrl: '',
                dialogVisible: false,
                
                rules: {
                    name: [{
                            required: true,
                            message: '请输入村庄名称',
                            trigger: 'blur'
                        },
                        {
                            min: 3,
                            message: '长度在3个字以上',
                            trigger: 'blur'
                        }
                    ],
                    address: [{
                        required: true,
                        message: '请选择地理位置',
                        trigger: 'change'
                    }],
                    
                }
            };
        },
        methods: {
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if(valid) {
                        axios.post('/api/add_village',this.ruleForm)
                        .then(function (response) {
                            console.log(response)
                        })
                        .catch(function (err) {
                            console.log(err)
                        })
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
            
            handleItemChange(val) {
                console.log('active item:', val);
                setTimeout(_ => {
                  if (val.indexOf('江苏') > -1 && !this.options2[0].cities.length) {
                    this.options2[0].cities = [{
                      label: '南京'
                    }];
                  } else if (val.indexOf('浙江') > -1 && !this.options2[1].cities.length) {
                    this.options2[1].cities = [{
                      label: '杭州'
                    }];
                  }
                }, 300);
            },
            
            resetForm(formName) {
                this.$refs[formName].resetFields();
            },
            
            handleUploadSuccess(file, fileList) {
                console.log(file)
                console.log(fileList)
            },
            
            handleRemove(file, fileList) {
                console.log(file, fileList);
            },
            handlePictureCardPreview(file) {
                this.dialogImageUrl = file.url;
                this.dialogVisible = true;
            }
        }
    }
</script>

<style scoped="scoped">
    .add-village {
        height: calc(100vh - 64px);
        padding: 20px;
        background-color: #fff;
    }
    .header {
        padding: 15px;
        padding-top: 30px;
    }
    .nav-bar {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .back-btn {
        margin-right: 40px;
        width: 100px;
        border-radius: 20px;
    }
</style>