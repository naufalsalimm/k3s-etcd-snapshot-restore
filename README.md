# 📦 Backup dan Restore Snapshot K3s

## 🗓️ Tanggal
11 April 2025

## 🧪 Lingkungan Uji
- Jenis cluster: K3s (R&D)
- Jumlah node: 1
- Versi: v1.31.3+k3s1

## 📌 Tujuan
Melakukan uji coba **restore cluster** menggunakan snapshot etcd dari direktori `/var/lib/rancher/k3s/etcd-snapshots`.

---

## 📂 Daftar Snapshot

```bash
ls -lh /var/lib/rancher/k3s/etcd-snapshots
```

Hasil:
```
-rw------- 1 root root 55M Apr 10 18:00 etcd-snapshot-server1-1744282803
-rw------- 1 root root 55M Apr 11 00:00 etcd-snapshot-server1-1744304401
-rw------- 1 root root 55M Apr 11 06:00 etcd-snapshot-server1-1744326002 ✅
```

---

## 🔁 Proses Restore

### 1. Hentikan service K3s
```bash
sudo systemctl stop k3s
```

### 2. Jalankan perintah restore dengan snapshot terakhir
```bash
sudo k3s server \
  --cluster-reset \
  --cluster-reset-restore-path=/var/lib/rancher/k3s/etcd-snapshots/etcd-snapshot-server1-1744326002
```

### 3. Log Output Penting
```
INFO[0013] Managed etcd cluster membership has been reset, restart without --cluster-reset flag now.
```

---

## 🚀 Restart dan Verifikasi

### 1. Restart service tanpa flag reset
```bash
sudo systemctl restart k3s
```

### 2. Verifikasi hasil restore
```bash
kubectl get nodes
kubectl get pods -A
kubectl get svc -A
```

---

## ✅ Hasil
- Cluster berhasil kembali ke state pada saat snapshot dibuat.
- Pod dan service kembali seperti sebelumnya.

## 📝 Catatan Tambahan
- Uji coba ini dilakukan pada lingkungan R&D (non-produksi).
- Snapshot otomatis disimpan oleh K3s sesuai konfigurasi default di direktori:  
  `/var/lib/rancher/k3s/etcd-snapshots`
