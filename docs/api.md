# Kontrak API Khawarizmi WorkOS

## Cara Membaca

- GET: melihat data. POST: membuat atau mengirim. PATCH: mengubah sebagian data.
- Tanda :id artinya nomor data tertentu, misalnya /api/projects/5.

## Kode Balasan

| Kode | Arti |
|---|---|
| 200 | Berhasil |
| 400 | Isi permintaan tidak lengkap atau salah bentuk |
| 401 | Belum login |
| 403 | Sudah login, tapi role tidak boleh melakukan aksi ini |
| 404 | Tidak ditemukan, juga dipakai untuk data milik pihak lain |
| 409 | Update bentrok, version sudah berubah (tantangan A2) |
| 422 | Dimengerti tapi tidak sah, misalnya perpindahan status yang salah atau alasan revisi kosong |

## Urutan Pemeriksaan di Server

1. Sudah login? Kalau belum: 401.
2. Berhak atas proyek data ini? Kalau tidak: 404.
3. Role boleh melakukan aksi ini? Kalau tidak: 403.
4. Isi permintaan sah, termasuk perpindahan status yang ada di tabel alur? Kalau tidak: 422.
5. version masih sama dengan di database? Kalau tidak: 409.
6. Kalau lolos semua: simpan perubahan dan tambah satu baris ActivityLog.

## Autentikasi

| Method dan alamat | Siapa boleh | Fungsi |
|---|---|---|
| POST /api/auth/login | Siapa saja | Masuk dengan email dan password |
| POST /api/auth/logout | Yang sudah login | Keluar |
| GET /api/auth/me | Yang sudah login | Lihat data diri sendiri |

## Proyek

| Method dan alamat | Siapa boleh | Fungsi |
|---|---|---|
| GET /api/projects | Yang sudah login | Daftar proyek yang berhak dilihat |
| POST /api/projects | Admin, PM | Buat proyek |
| GET /api/projects/:id | Anggota proyek, atau Client pemilik | Detail satu proyek |
| POST /api/projects/:id/members | Admin, PM pengelola proyek itu | Tambah anggota |

## Task

| Method dan alamat | Siapa boleh | Fungsi |
|---|---|---|
| GET /api/projects/:id/tasks | Anggota proyek, atau Client pemilik | Daftar task satu proyek |
| POST /api/projects/:id/tasks | Admin, PM pengelola proyek itu | Buat dan assign task |
| GET /api/tasks/:id | Anggota proyek, atau Client pemilik | Detail satu task |
| PATCH /api/tasks/:id/status | Developer yang di-assign | Pindah status (To Do, In Progress, In Review, Revision ke In Progress) |
| POST /api/tasks/:id/comments | Anggota proyek, atau Client pemilik | Tulis komentar |

Isi permintaan PATCH /api/tasks/:id/status:

```json
{
  "toStatus": "In Review",
  "version": 3
}
```

## Review Klien

| Method dan alamat | Siapa boleh | Fungsi |
|---|---|---|
| POST /api/tasks/:id/approve | Client pemilik proyek | Setujui task, hanya dari In Review |
| POST /api/tasks/:id/request-revision | Client pemilik proyek | Minta revisi, reason wajib diisi |
| GET /api/tasks/:id/approvals | Anggota proyek, atau Client pemilik | Riwayat semua keputusan task |

## Notifikasi, Audit Log, Dashboard

| Method dan alamat | Siapa boleh | Fungsi |
|---|---|---|
| GET /api/notifications | Yang sudah login | Notifikasi milik sendiri |
| PATCH /api/notifications/:id/read | Pemilik notifikasi | Tandai sudah dibaca |
| GET /api/projects/:id/activity | Admin, PM pengelola proyek itu | Audit log satu proyek |
| GET /api/dashboard | Yang sudah login | Progres, task terlambat, pekerjaan aktif sesuai hak akses |

## Catatan Desain

- Tidak ada endpoint untuk mengubah atau menghapus audit log. Riwayat tidak boleh diutak-atik.
- Approve dan minta revisi punya endpoint sendiri karena keduanya juga menambah satu baris di tabel Approval.
- Skenario ujian: Approved lalu Developer mencoba kembali ke In Progress. Perpindahan itu tidak ada di tabel alur, jadi server menolak dengan 422 dan tidak ada yang berubah.