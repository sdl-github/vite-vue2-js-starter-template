<template>
  <el-dialog
    top="1vh"
    :title="currentId ? '编辑' : '新增'"
    :visible="value"
    width="800px"
    :close-on-click-modal="false"
    :before-close="handleClose"
  >
    <el-form ref="baseForm" :model="baseForm" size="small" label-width="100px">
      <el-row>
        <el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24">
          <el-form-item label="关联合同" prop="contractNo">
            <el-input v-model="baseForm.contractNo" style="width: 600px" />
          </el-form-item>
        </el-col>
        <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12">
          <el-form-item label="状态" prop="contractNo">
            <el-input v-model="baseForm.status" style="width: 220px" />
          </el-form-item>
        </el-col>
        <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12">
          <el-form-item label="船名" prop="contractNo">
            <el-input v-model="baseForm.shipName" style="width: 220px" />
          </el-form-item>
        </el-col>
        <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12">
          <el-form-item label="航次号" prop="contractNo">
            <el-input v-model="baseForm.voyageNo" style="width: 220px" />
          </el-form-item>
        </el-col>
        <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12">
          <el-form-item label="贸易类型" prop="contractNo">
            <el-input v-model="baseForm.type" style="width: 220px" />
          </el-form-item>
        </el-col>
      </el-row>
    </el-form>

    <div class="part-wapper">
      <div class="main-title">装卸信息</div>
      <el-form ref="cargeForm" :model="cargeForm" size="small" label-width="100px">
        <el-row>
          <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12">
            <el-form-item label="货物名称" prop="contractNo">
              <el-input v-model="cargeForm.name" style="width: 220px" />
            </el-form-item>
          </el-col>
          <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12">
            <el-form-item label="货物数量" prop="contractNo">
              <el-input v-model="cargeForm.num" style="width: 220px" />
            </el-form-item>
          </el-col>
          <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12">
            <el-form-item label="航次指令" prop="contractNo">
              <el-input v-model="cargeForm.code" style="width: 220px" />
            </el-form-item>
          </el-col>
          <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12">
            <el-form-item label="是否洗舱" prop="contractNo">
              <el-input v-model="cargeForm.isWash" style="width: 220px" />
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>

      <div class="sub-title">装货港与时间</div>
      <el-form ref="cargeForm" :model="loadForm" size="small" label-width="100px">
        <el-row>
          <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12">
            <el-form-item label="装货港口1" prop="contractNo">
              <el-input v-model="loadForm.first.port" style="width: 220px" />
            </el-form-item>
          </el-col>
          <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12">
            <el-form-item label="到达装港时间1" prop="contractNo">
              <div style="display: flex; justify-content: space-between">
                <el-input v-model="loadForm.first.time" style="width: 200px" />
                <el-button type="primary" @click="handleAddLoadPort">添加</el-button>
              </div>
            </el-form-item>
          </el-col>
          <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12">
            <el-form-item label="装货港口1" prop="contractNo">
              <el-input v-model="loadForm.first.port" style="width: 220px" />
            </el-form-item>
          </el-col>
          <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12">
            <el-form-item label="到达装港时间1" prop="contractNo">
              <div style="display: flex; justify-content: space-between">
                <el-input v-model="loadForm.first.time" style="width: 200px" />
                <el-button type="primary" @click="handleAddLoadPort">添加</el-button>
              </div>
            </el-form-item>
          </el-col>
          <template v-for="(item, index) in loadForm.dynamic">
            <el-col :key="index" :xs="12" :sm="12" :md="12" :lg="12" :xl="12">
              <el-form-item :label="'装货港口' + (index + 2)" prop="contractNo">
                <el-input v-model="item.port" style="width: 220px" />
              </el-form-item>
            </el-col>
            <el-col :key="index" :xs="12" :sm="12" :md="12" :lg="12" :xl="12">
              <el-form-item :label="'到达装港时间' + (index + 2)" prop="contractNo">
                <div style="display: flex; justify-content: space-between">
                  <el-input v-model="item.time" style="width: 200px" />
                  <el-button type="primary">删除</el-button>
                </div>
              </el-form-item>
            </el-col>
          </template>
        </el-row>
      </el-form>
      <div class="sub-title">卸货港与时间</div>
    </div>

    <span slot="footer" class="dialog-footer">
      <el-button @click="cancle">取消</el-button>
      <el-button type="primary" @click="ok">保存</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  props: {
    value: {
      type: Boolean,
      default: false,
    },
    currentId: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      baseForm: {},
      cargeForm: {},
      loadForm: {
        first: {},
        dynamic: [],
      },
    };
  },
  watch: {
    currentId(val) {
      if (val) {
        this.initData();
      }
    },
  },
  methods: {
    initData() {},
    handleClose() {
      this.baseForm = {};
      this.cargeForm = {};
      this.loadForm = {
        first: {},
        dynamic: [],
      };
      this.$emit('input', false);
    },
    cancle() {
      this.handleClose();
    },
    ok() {
      this.handleClose();
    },
    handleAddLoadPort() {
      this.loadForm.dynamic.push({
        port: '',
        time: '',
      });
    },
  },
};
</script>

<style lang="scss" scoped>
.part-wapper {
  .main-title {
    background-color: #f2f2f2;
    margin-bottom: 20px;
    padding: 0 0 0 10px;
    width: 100%;
    height: 35px;
    line-height: 35px;
    font-size: 15px;
    font-weight: bolder;
  }

  .sub-title {
    margin-bottom: 20px;
    padding: 0 0 0 10px;
    font-size: 15px;
    font-weight: bolder;
  }
}
</style>
