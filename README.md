# HEAF496 — Website preview

Website statis responsif untuk HEAF496, dibuat dengan HTML, CSS, dan JavaScript tanpa proses build.

## Menjalankan
Buka `index.html` langsung atau jalankan server di folder ini: `python -m http.server 8000`, lalu buka http://localhost:8000.

Area admin ada pada footer. Sandi preview: `C1R4ngrang496`. Pesanan dan konsultasi tersimpan dalam localStorage browser yang sama. Ini hanya demonstrasi lokal: login diperiksa di JavaScript, tidak ada backend, dan data tidak terkirim ke server. File NPWP pada formulir belum diunggah. Harga dan gambar adalah contoh.

## GitHub
```bash
git init
git add .
git commit -m "Build HEAF496 website preview"
git branch -M main
git remote add origin https://github.com/USERNAME/heaf496.git
git push -u origin main
```
Buat repository GitHub terlebih dahulu dan ganti URL remote dengan repository Anda.

## Deploy Vercel / Netlify
Impor repository, pilih Other/Static, build command kosong, dan publish directory folder berisi `index.html` (root jika isi folder ini menjadi root repo). Untuk backend Next.js/Supabase, tambahkan environment variables pada pengaturan hosting.

## Supabase SQL starter
Sebelum go-live, gunakan Supabase Auth, validasi server-side dan RLS. Jangan gunakan sandi demo. Simpan berkas usaha pada storage privat dan gunakan tautan bertanda tangan untuk admin.
```sql
create extension if not exists pgcrypto;
create table public.products(id uuid primary key default gen_random_uuid(),name text not null,description text not null default '',price_idr bigint not null check(price_idr>=0),unit text not null default 'kg',stock integer not null default 0,image_url text,is_published boolean not null default false,created_at timestamptz not null default now());
create table public.orders(id uuid primary key default gen_random_uuid(),customer_name text not null,whatsapp text not null,delivery_address text not null,product_name text not null,quantity numeric(10,2) not null check(quantity>0),note text,status text not null default 'baru',created_at timestamptz not null default now());
create table public.web_client_leads(id uuid primary key default gen_random_uuid(),email text not null,contact_name text not null,phone text not null,product_description text not null,business_document_path text,created_at timestamptz not null default now());
create table public.articles(id uuid primary key default gen_random_uuid(),title text not null,slug text unique not null,category text not null,excerpt text not null default '',body text not null default '',is_published boolean not null default false,created_at timestamptz not null default now());
create table public.gallery_items(id uuid primary key default gen_random_uuid(),title text not null,media_url text not null,media_type text not null check(media_type in ('image','video')),caption text,is_published boolean not null default false,created_at timestamptz not null default now());
create table public.contact_messages(id uuid primary key default gen_random_uuid(),name text not null,whatsapp text,email text,message text not null,created_at timestamptz not null default now());
create table public.site_settings(key text primary key,value jsonb not null default '{}',updated_at timestamptz not null default now());
-- Enable RLS on every table. Add narrowly scoped policies by role.
-- Never expose public SELECT on orders, leads, or contact_messages.
```

## Identitas
HEAF496 — Solusi Digital • Bisnis Daging Sapi • Konten Inspiratif. Haji Endang Achmad Fauzi. WhatsApp 0859 4203 8328. Jl. Kopo Cirangrang No 496, Bandung, 40225.

## Halaman admin terpisah
Website publik berada di `index.html`; dashboard berada di `admin.html`. Jalankan server lokal dari folder proyek (misalnya `python -m http.server 8000`) agar kedua halaman berbagi localStorage pada origin yang sama. Admin dapat menambahkan catatan internal, memperbarui status, menulis balasan/resi, menghubungi pemesan via WhatsApp, dan mengekspor rekap CSV. Untuk keamanan data dan agar pelanggan benar-benar menerima balasan/resi, backend dan autentikasi server-side tetap perlu disambungkan.

Logo utama yang Anda berikan tersimpan sebagai `logo-heaf496.png` dan tampil di kanan atas header.

## Upload file melalui Admin
Menu **Kategori & Konten** memungkinkan admin menambah kategori dan isinya dengan pemilih file gambar/video/PDF. Form produk juga menerima file gambar. File demo dibatasi 650 KB per file dan disimpan di localStorage browser, bukan server; jalankan `python -m http.server 8000` lalu buka website/admin melalui localhost agar penyimpanan bersama. Produk, konten, kategori, pesanan, catatan dan status demo disimpan per browser. Gunakan backend + storage privat untuk operasional nyata.
