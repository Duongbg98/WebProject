<template>
  <div>
    <div
      id="dlgEmployeeDetail"
      class="dialog"
      :class="{ 'dialog-hide': !isShow }"
    >
      <div class="model"></div>
      <div class="dialog-content">
        <div class="dialog-header">
          <div class="dialog-title">THÔNG TIN NHÂN VIÊN</div>
          <div class="dialog-close-button" @click="btnCloseClickOn()">
            &#x2715;
          </div>
        </div>
        <div class="dialog-body">
          <div class="m-image">
            <img url="../../assets/img/toggle.png" />
            <label>(Vui lòng chọn ảnh có định dạng</label>
            <label>.jpg, .jpeg, .png, .gif)</label>
          </div>
          <p>A.THÔNG TIN CHUNG</p>
          <div class="m-row">
            <div class="m-col required">
              <label>Mã nhân viên (<a style="color: red">*</a>)</label>
              <input
                autofocus
                id="txtEmployeeCode"
                type="text"
                title="Không để trống trường này"
                v-model="employee.employeeCode"
              />
            </div>
            <div class="m-col required">
              <label>Họ và tên (<a style="color: red">*</a>)</label>
              <input
                id="txtFullName"
                type="text"
                title="Không để trống trường này"
                v-model="employee.FullName"
              />
            </div>
          </div>
          <div class="m-row">
            <div class="m-col">
              <label>Ngày sinh</label>
              <input
                id="dtDateOfBirth"
                type="date"
                v-model="employee.DateOfBirth"
              />
            </div>
            <div class="m-col">
              <label>Giới tính</label>
              <select id="cbGender" v-model="employee.Gender">
                <option value="1">Nam</option>
                <option value="0">Nữ</option>
                <option value="2">Không xác định</option>
              </select>
            </div>
          </div>
          <div class="m-row">
            <div class="m-col required">
              <label>Số CMTND/Căn cước (<a style="color: red">*</a>)</label>
              <input
                id="txtIdentityNumber"
                type="text"
                title="Không để trống trường này"
                v-model="employee.IdentityNumber"
              />
            </div>
            <div class="m-col">
              <label>Ngày cấp</label>
              <input
                id="dtIdentityDate"
                type="date"
                v-model="employee.IdentityDate"
              />
            </div>
          </div>
          <div class="m-row">
            <div class="m-col">
              <label>Nơi cấp</label>
              <input
                id="txtIdentityPlace"
                type="text"
                v-model="employee.IdentityPlace"
              />
            </div>
          </div>
          <div class="m-row">
            <div class="m-col required">
              <label>Email (<a style="color: red">*</a>)</label>
              <input
                id="txtEmail"
                type="text"
                title="Không để trống trường này"
                v-model="employee.Email"
              />
            </div>
            <div class="m-col required">
              <label>Số điện thoại (<a style="color: red">*</a>)</label>
              <input
                id="txtPhoneNumber"
                type="text"
                title="Không để trống trường này"
                v-model="employee.PhoneNumber"
              />
            </div>
          </div>
          <p>B.THÔNG TIN CÔNG VIỆC</p>
          <div class="m-row">
            <div class="m-col">
              <label>Vị trí</label>
              <select id="txtPositionName" v-model="employee.PositionId">
                <option value="3700cc49-55b5-69ea-4929-a2925c0f334d">
                  Giám đốc
                </option>
                <option value="148ed882-32b8-218e-9c20-39c2f00615e8">
                  Nhân viên
                </option>
                <option value="25c6c36e-1668-7d10-6e09-bf1378b8dc91">
                  Thu ngân
                </option>
              </select>
            </div>
            <div class="m-col">
              <label>Phòng ban</label>
              <select id="txtDepartmentName" v-model="employee.DepartmentId">
                <option value="17120d02-6ab5-3e43-18cb-66948daf6128">
                  Phòng đào tạo
                </option>
                <option value="469b3ece-744a-45d5-957d-e8c757976496">
                  Phòng nhân sự
                </option>
                <option value="142cb08f-7c31-21fa-8e90-67245e8b283e">
                  Phòng Marketing
                </option>
                <option value="4e272fc4-7875-78d6-7d32-6a1673ffca7c">
                  Phòng Công nghệ
                </option>
              </select>
            </div>
          </div>
          <div class="m-row">
            <div class="m-col">
              <label>Mã số thuế cá nhân</label>
              <input
                id="txtPersonalTaxCode"
                type="text"
                v-model="employee.PersonalTaxCode"
              />
            </div>
            <div class="m-col">
              <label>Mức lương cơ bản</label>
              <input id="txtSalary" type="text" v-model="employee.Salary" />
            </div>
          </div>
          <div class="m-row">
            <div class="m-col">
              <label>Ngày gia nhập công ty</label>
              <input id="dtJoinDate" type="date" v-model="employee.JoinDate" />
            </div>
            <div class="m-col">
              <label>Tình trạng công việc</label>
              <input
                id="txtWorkStatus"
                type="text"
                v-model="employee.WorkStatus"
              />
            </div>
          </div>
        </div>
        <div class="dialog-footer">
          <label class="cancel" @click="btnCloseClickOn()">Hủy</label>
          <button
            id="btnSave"
            class="btn-default btn-add"
            @click="btnSaveOnClick"
          >
            Lưu
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  props: {
    isShow: { type: Boolean, default: false },
    employee: { type: Object, default: null },
    formMode: { type: String, default: "add"},
    
  },
  created() {

  },
  watch: {
    employeeId: function () {
      // Lấy dữ liệu từ Api:
      axios
        .get("http://api.manhnv.net/v1/Employees" + this.employeeId)
        .then((response) => {
          this.employee = response.data;
        })
        .catch((res) => {
          alert(res.status);
        });
    },
  },
  methods: {
    // Ẩn dialog 
    btnCloseClickOn() {
      this.$emit('hideDialog');
    },
    btnSaveOnClick() {
      // Lấy id của khách hàng:      
      // Kiểm tra trạng thái của form (là thêm mới hay là sửa):
      // Gọi service thực hiện cất dữ liệu:
      if (this.formMode == "add") {
        axios.post("http://api.manhnv.net/v1/Employees/", this.employee)
        .then(response => {console.log(response);  this.$emit('hideDialog');})
        .catch(response => {console.log(response)});
        
      } else {
        axios.put("http://api.manhnv.net/v1/Employees/" + this.employee.EmployeeId,this.employee)
        .then(response => {console.log(response);  this.$emit('hideDialog');})
        .catch(response => {console.log(response)});
      }      
    },
    validateEmail: function(email) {
            const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
            return !re.test(String(email).toLowerCase());
        }
  },
  data() {
    return {
      
    };
  },
}; 
</script>
<style scoped>
</style>