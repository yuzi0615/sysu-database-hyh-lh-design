<template>
    <div class="shop-container">
        <div class="header">
            <h2>欢迎点餐</h2>
            <p class="header-subtitle">选择您喜欢的美食，享受校园美食之旅</p>
        </div>
        
        <div class="body">
            <!-- 搜索和筛选 -->
            <div class="filter-section">
                <el-input 
                    placeholder="搜索店铺" 
                    prefix-icon="el-icon-search"
                    class="search-input"
                    v-model="searchQuery"
                    @input="handleSearch"
                ></el-input>
                <el-select 
                    v-model="sortBy" 
                    placeholder="排序方式"
                    class="sort-select"
                >
                    <el-option label="默认排序" value="default"></el-option>
                    <el-option label="销量最高" value="sale"></el-option>
                    <el-option label="价格最低" value="price"></el-option>
                </el-select>
            </div>
            
            <!-- 店铺卡片列表 -->
            <div class="shop-grid">
                <div 
                    v-for="shop in filteredShops" 
                    :key="shop.shop_name"
                    class="shop-card"
                    @click="showdia(shop)"
                >
                    <div class="shop-card-header">
                        <h3 class="shop-name">{{ shop.shop_name }}</h3>
                        <div class="shop-sale">
                            <i class="el-icon-s-order"></i>
                            <span>月售 {{ shop.sale }}</span>
                        </div>
                    </div>
                    <div class="shop-card-body">
                        <div class="shop-price">
                            <span class="price-label">单价：</span>
                            <span class="price-value">¥{{ shop.price }}</span>
                        </div>
                    </div>
                    <div class="shop-card-footer">
                        <el-button type="primary" size="small" class="order-btn">
                            立即订餐
                        </el-button>
                    </div>
                </div>
            </div>

            <!-- 订餐对话框 -->
            <el-dialog 
                title="订餐表单" 
                :visible.sync="dialog" 
                class="custom-dialog" 
                width="450px"
                top="15vh"
            >
                <div>
                    <el-form ref="form" :model="form" label-width="100px" class="order-form">
                        <el-form-item label="店铺名称：">
                            <span class="form-value">{{ form.shop_name }}</span>
                        </el-form-item>

                        <el-form-item label="产品单价：">
                            <span class="form-value">¥{{ form.order_money }}</span>
                        </el-form-item>

                        <el-form-item label="订餐方式：">
                            <el-select v-model="form.order_way" placeholder="请选择订餐方式" class="custom-select">
                                <el-option label="外卖配送" value="外卖配送"></el-option>
                                <el-option label="线下自取" value="线下自取"></el-option>
                            </el-select>
                        </el-form-item>

                        <el-form-item label="客户姓名：">
                            <el-input v-model="form.cons_name" class="custom-input"></el-input>
                        </el-form-item>

                        <el-form-item label="送餐地址：">
                            <el-input v-model="form.cons_addre" class="custom-input"></el-input>
                        </el-form-item>

                    </el-form>
                    <div class="dialog-footer">
                        <el-button @click="dialog = false">取消</el-button>
                        <el-button type="primary" @click="add" class="submit-btn">
                            提交订单
                        </el-button>
                    </div>
                </div>
            </el-dialog>
        </div>
    </div>
</template>

<script>
export default {
    created() {
        this.getdata();
    },
    data() {
        return {
            tableData: [],
            dialog: false,
            searchQuery: '',
            sortBy: 'default',
            form: {
                shop_name: '',
                order_money: '',
                order_way: '',
                cons_name: '',
                cons_addre: '',
            }
        }
    },
    computed: {
        filteredShops() {
            let shops = [...this.tableData];
            
            // 搜索过滤
            if (this.searchQuery) {
                const query = this.searchQuery.toLowerCase();
                shops = shops.filter(shop => 
                    shop.shop_name.toLowerCase().includes(query)
                );
            }
            
            // 排序
            if (this.sortBy === 'sale') {
                shops.sort((a, b) => b.sale - a.sale);
            } else if (this.sortBy === 'price') {
                shops.sort((a, b) => a.price - b.price);
            }
            
            return shops;
        }
    },
    methods: {
        getdata() {
            this.$axios.get("/api/user/shop").then((res) => {
                console.log(res.data);
                if (res.data.status == 200) {
                    this.tableData = res.data.tabledata;
                }
            })
        },
        showdia(row) {
            this.form.shop_name = row.shop_name;
            this.form.order_money = row.price;
            this.form.order_way = '外卖配送'; // 默认选择外卖配送
            this.dialog = true;
        },
        add() {
            // 表单验证
            if (!this.form.cons_name) {
                this.$message.error('请输入客户姓名');
                return;
            }
            if (!this.form.cons_addre) {
                this.$message.error('请输入送餐地址');
                return;
            }
            if (!this.form.order_way) {
                this.$message.error('请选择订餐方式');
                return;
            }
            
            this.$axios.post("/api/user/addorder", this.form).then((res) => {
                console.log(res.data);
                if (res.data.status == 200) {
                    this.$message({
                        message: "成功下单",
                        type: "success"
                    })
                    this.dialog = false;
                    this.getdata();
                } else {
                    this.$message.error(res.data.msg || '下单失败，请重试');
                }
            }).catch(() => {
                this.$message.error('网络错误，请稍后重试');
            })
        },
        handleSearch() {
            // 搜索逻辑已在computed中处理
        }
    }
}
</script>

<style scoped>
.shop-container {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
}

/* 头部样式 */
.header {
    text-align: center;
    padding: 30px 0;
    margin-bottom: 20px;
    background-color: white;
    border-radius: 12px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
    
    h2 {
        color: #333;
        font-size: 28px;
        font-weight: 600;
        margin: 0 0 8px;
    }
    
    .header-subtitle {
        color: #666;
        font-size: 14px;
        margin: 0;
    }
}

/* 主体内容 */
.body {
    flex: 1;
    padding: 0 20px 20px;
    overflow-y: auto;
}

/* 搜索和筛选区域 */
.filter-section {
    display: flex;
    gap: 16px;
    margin-bottom: 30px;
    padding: 20px;
    background-color: white;
    border-radius: 12px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
}

.search-input {
    flex: 1;
    max-width: 400px;
    border-radius: 8px;
    transition: all 0.3s ease;
    
    &:focus {
        box-shadow: 0 0 0 2px rgba(102, 126, 234, 0.2);
    }
}

.sort-select {
    width: 160px;
    border-radius: 8px;
    transition: all 0.3s ease;
    
    &:focus {
        box-shadow: 0 0 0 2px rgba(102, 126, 234, 0.2);
    }
}

/* 店铺网格 */
.shop-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 20px;
}

/* 店铺卡片 */
.shop-card {
    background-color: white;
    border-radius: 12px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
    padding: 20px;
    transition: all 0.3s ease;
    cursor: pointer;
    
    &:hover {
        transform: translateY(-5px);
        box-shadow: 0 5px 16px rgba(0, 0, 0, 0.12);
    }
}

.shop-card-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 16px;
    
    .shop-name {
        font-size: 18px;
        font-weight: 600;
        color: #333;
        margin: 0;
        flex: 1;
    }
    
    .shop-sale {
        display: flex;
        align-items: center;
        gap: 4px;
        background-color: rgba(102, 126, 234, 0.1);
        color: #667eea;
        padding: 4px 12px;
        border-radius: 12px;
        font-size: 12px;
        font-weight: 500;
    }
}

.shop-card-body {
    margin-bottom: 20px;
    
    .shop-price {
        display: flex;
        align-items: baseline;
        gap: 8px;
        
        .price-label {
            color: #666;
            font-size: 14px;
        }
        
        .price-value {
            color: #ff6b6b;
            font-size: 24px;
            font-weight: 600;
        }
    }
}

.shop-card-footer {
    display: flex;
    justify-content: center;
}

.order-btn {
    background-color: #667eea;
    border-color: #667eea;
    border-radius: 8px;
    transition: all 0.3s ease;
    
    &:hover {
        background-color: #5a6fd8;
        border-color: #5a6fd8;
        transform: translateY(-1px);
    }
}

/* 订餐对话框 */
.custom-dialog {
    border-radius: 12px;
    overflow: hidden;
    
    .el-dialog__header {
        background-color: #667eea;
        color: white;
        padding: 20px 24px;
        
        .el-dialog__title {
            color: white;
            font-size: 18px;
            font-weight: 600;
        }
        
        .el-dialog__headerbtn .el-dialog__close {
            color: white;
            
            &:hover {
                color: rgba(255, 255, 255, 0.8);
            }
        }
    }
    
    .el-dialog__body {
        padding: 24px;
    }
}

.order-form {
    .form-value {
        font-size: 14px;
        color: #333;
        font-weight: 500;
    }
    
    .custom-input {
        border-radius: 8px;
        transition: all 0.3s ease;
        
        &:focus {
            box-shadow: 0 0 0 2px rgba(102, 126, 234, 0.2);
        }
    }
    
    .custom-select {
        width: 100%;
        border-radius: 8px;
        transition: all 0.3s ease;
        
        &:focus {
            box-shadow: 0 0 0 2px rgba(102, 126, 234, 0.2);
        }
    }
}

.dialog-footer {
    display: flex;
    justify-content: flex-end;
    gap: 12px;
    margin-top: 30px;
    padding-top: 20px;
    border-top: 1px solid #f0f0f0;
    
    .submit-btn {
        background-color: #667eea;
        border-color: #667eea;
        border-radius: 8px;
        transition: all 0.3s ease;
        
        &:hover {
            background-color: #5a6fd8;
            border-color: #5a6fd8;
        }
    }
}

/* 响应式设计 */
@media (max-width: 768px) {
    .filter-section {
        flex-direction: column;
        align-items: stretch;
    }
    
    .search-input {
        max-width: none;
    }
    
    .sort-select {
        width: 100%;
    }
    
    .shop-grid {
        grid-template-columns: 1fr;
    }
    
    .header h2 {
        font-size: 24px;
    }
}
</style>