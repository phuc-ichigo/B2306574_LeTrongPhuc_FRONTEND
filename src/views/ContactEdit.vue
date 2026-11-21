<template>
  <div class="container mt-4">
    <div class="row justify-content-center">
      <div class="col-md-6">
        <h3>Chỉnh sửa liên hệ</h3>
        <form @submit.prevent="updateContact">
          <div class="form-group">
            <label>Họ tên</label>
            <input
              v-model="contact.name"
              type="text"
              class="form-control"
              placeholder="Nhập họ tên"
              required
            />
          </div>
          <div class="form-group mt-2">
            <label>Email</label>
            <input
              v-model="contact.email"
              type="email"
              class="form-control"
              placeholder="Nhập email"
            />
          </div>
          <div class="form-group mt-2">
            <label>Địa chỉ</label>
            <input
              v-model="contact.address"
              type="text"
              class="form-control"
              placeholder="Nhập địa chỉ"
            />
          </div>
          <div class="form-group mt-2">
            <label>Số điện thoại</label>
            <input
              v-model="contact.phone"
              type="text"
              class="form-control"
              placeholder="Nhập số điện thoại"
            />
          </div>
          <div class="mt-3">
            <button type="submit" class="btn btn-primary mr-2">Cập nhật</button>
            <router-link to="/" class="btn btn-secondary">Hủy</router-link>
          </div>
        </form>
        <div v-if="message" class="alert alert-info mt-3">{{ message }}</div>
      </div>
    </div>
  </div>
</template>

<script>
import ContactService from "@/services/contact.service";

export default {
  name: "ContactEdit",
  data() {
    return {
      contact: {
        name: "",
        email: "",
        address: "",
        phone: "",
      },
      message: "",
    };
  },
  async created() {
    const id = this.$route.params.id;
    try {
      const response = await ContactService.get(id);
      this.contact = response.data;
    } catch (error) {
      this.message = "Không tìm thấy liên hệ.";
    }
  },
  methods: {
    async updateContact() {
      const id = this.$route.params.id;
      try {
        await ContactService.update(id, this.contact);
        this.message = "Cập nhật thành công!";
        this.$router.push("/");
      } catch (error) {
        this.message = "Có lỗi xảy ra: " + error.message;
      }
    },
  },
};
</script>
