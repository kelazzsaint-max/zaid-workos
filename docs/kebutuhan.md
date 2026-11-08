\# Kebutuhan Sistem



\## Matriks Role x Aksi



| Aksi | Admin | PM | Developer | Client |

|---|---|---|---|---|

| Buat proyek | Ya | Ya | Tidak | Tidak |

| Tambah anggota proyek | Ya | Ya,hanya di proyek yang dia kelola | Tidak | Tidak |

| Lihat proyek | Ya,semua | Ya,Proyek yang ia ikuti | Ya,proyek yang ia ikuti | Ya,hanya proyek miliknya | 

| Buat dan assign task | Ya | Ya,di proyeknya | Tidak | Tidak |

| Ubah status ke In Progress / In Review | Tidak | Tidak | Ya,hanya task yang di assign ke dia | Tidak |

| Approve / minta revisi | Tidak | Tidak | Tidak | Ya,hanya task di proyeknya,dan revisi wajib ada alasan |

| Lihat audit log | Ya,semua | Ya,proyek yang ia kelola | Tidak | Tidak |



\## Alur Status Task



Status yang ada: To Do, In Progress, In Review, Approved, Revision.



| Dari | Ke | Siapa yang boleh | Syarat |

|---|---|---|---|

| To Do | In Progress | Developer yang di-assign | - |

| In Progress | In Review | Developer yang di-assign | - |

| In Review | Approved | Client pemilik proyek | - |

| In Review | Revision | Client pemilik proyek | Alasan wajib diisi |

| Revision | In Progress | Developer yang di-assign | - |



Aturan penting:



\- Approved adalah status akhir. Tidak ada perpindahan keluar dari Approved.

\- Semua perpindahan di luar tabel di atas ditolak oleh server.

\- Contoh yang ditolak: To Do langsung ke Approved, dan Approved kembali ke In Progress.



\## Task yang Sudah Approved



Task yang sudah Approved tidak bisa dibuka kembali. Kalau ada kesalahan, buat task baru. Alasannya: menjaga riwayat keputusan klien tetap utuh.

