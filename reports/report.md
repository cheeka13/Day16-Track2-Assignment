# Báo cáo Benchmark: CPU Instance với LightGBM trên GCP

## Kết quả Training và Inference

**Machine Type:** n2-standard-8 (8 vCPU, 32GB RAM)  
**Dataset:** Credit Card Fraud Detection (284,807 giao dịch)  
**Model:** LightGBM Classifier

### Hiệu suất Training:
- **Thời gian load data:** 2.18 giây
- **Thời gian training:** 1.32 giây (cực nhanh nhờ 8 vCPU)
- **AUC-ROC:** 0.8213 (tốt cho bài toán fraud detection)
- **Accuracy:** 99.88% (cao do dataset imbalanced)
- **F1-Score:** 0.6497 (cân bằng precision/recall)

### Hiệu suất Inference:
- **Single row latency:** 1.26ms (rất nhanh)
- **Throughput:** 435,274 rows/sec (hiệu suất cao)

## Lý do sử dụng CPU thay vì GPU

**Tài khoản GCP mới bị hạn chế quota GPU T4 = 0.** Thay vì chờ duyệt quota (có thể bị từ chối), phương án CPU n2-standard-8 mang lại:
1. **Không cần quota đặc biệt** - khả dụng ngay
2. **Chi phí thấp hơn** - ~$0.43/giờ vs ~$0.54/giờ (GPU + VM)
3. **Hiệu suất tốt** - LightGBM tối ưu cho CPU, training chỉ 1.32 giây
4. **Inference nhanh** - 1.26ms latency, 435K rows/sec throughput

**Kết luận:** Với workload Machine Learning truyền thống (không phải Deep Learning), CPU instance cao cấp thường hiệu quả và tiết kiệm hơn GPU.