<template>
    <div class="app-container">
        <div class="el-toolbar">
            <div class="el-toolbar-body" style="justify-content: flex-start;">
                <a href="http://localhost:8202/admin/cmn/dict/exportData" target="_blank">
                    <el-button type="primary"><i class="el-icon--left el-icon-download"></i>下载</el-button>
                </a>
                <el-button type="primary" @click="importData"><i class="el-icon--left el-icon-upload2"/>上传</el-button>
            </div>
        </div>

        <el-table
        :data="list"
        style="width: 100%"
        row-key="id"
        border
        lazy
        :load="getChildren"
        :tree-props="{children: 'children', hasChildren: 'hasChildren'}">

            <el-table-column label="名称" width="230" align="left">
            <template slot-scope="scope">
            <span>{{ scope.row.name }}</span>
            </template>
            </el-table-column>
            <el-table-column label="编码" width="220">
            <template slot-scope="{row}">
                    {{ row.dictCode }}
            </template>
            </el-table-column>
            <el-table-column label="值" width="230" align="left">
            <template slot-scope="scope">
            <span>{{ scope.row.value }}</span>
            </template>
            </el-table-column>

            <el-table-column label="创建时间" align="center">
            <template slot-scope="scope">
            <span>{{ scope.row.createTime }}</span>
            </template>
            </el-table-column>
        </el-table>

        <el-dialog title="上传" :visible.sync="dialogImportVisible" width="480px">
            <el-form label-position="right" label-width="170px">

            <el-form-item label="文件">

            <el-upload
            :multiple="false"
            :on-success="onUploadSuccess"
            :action="'http://localhost:8202/admin/cmn/dict/importData'"
            class="upload-demo">
            <el-button size="small" type="primary">点击上传</el-button>
            <div slot="tip" class="el-upload__tip">只能上传excel文件，且不超过500kb</div>
            </el-upload>

            </el-form-item>

            </el-form>
            <div slot="footer" class="dialog-footer">
            <el-button @click="dialogImportVisible = false">
                取消
            </el-button>
            </div>
        </el-dialog>

    </div>

</template>
<script>
import dict from '@/api/dict'
export default {
    data() {
        return {
            //设置弹框是否弹出 false为不弹出
            dialogImportVisible:false,
            list:[] //数据字典列表数组
        }
    },
    created() {
        this.getDictList(1)
    },
    methods: {
        //上传数据字典
        importData() {
            this.dialogImportVisible = true
        },
        //上传成功调用
        onUploadSuccess() {
            //关闭弹框
            this.dialogImportVisible = false
            //刷新页面
            this.getDictList(1)
        },
        //下载数据字典
        exportData() {
            //调用导出接口
            window.location.href="http://localhost:8202/admin/cmn/dict/exportData"
        },
        //数据字典列表
        getDictList(id) {
            dict.dictList(id)
                .then(response => {
                    this.list = response.data
                })
        },
        //层级显示
        getChildren(tree, treeNode, resolve) {
            dict.dictList(tree.id).then(response => {
                resolve(response.data)
            })
        }
    }
}
</script>