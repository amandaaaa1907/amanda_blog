---
title: "Implementing DevSecOps using GitLab SAST in CI Pipeline"
layout: post
date: 2024-11-22 15:17
image: /assets/images/markdown.jpg
headerImage: false
tag:
- devsecops
- gitlab
- sast
category: blog
author: amanda
description: Implementing DevSecOps
---

Haloo temen temen semuanya👋👋 Jadi di Blog ini merupakan dokumentasi tentang cara menerapkan konsep *DevSecOps* dengan memanfaatkan GitLab SAST (Static Application Security Testing) di CI Pipeline. Di sini, akan membahas langkah-langkah untuk mengintegrasikan keamanan aplikasi ke dalam pengembangan software.
<p>Udah penasaran???? Yukk langsung aja yukk!!</p>

## Deskripsi
<p>Dalam Era digitalisasi yang terus berkembang, keamanan perangkat lunak menjadi aspek
yang sangat penting bagi perusahaan. Untuk itu, tim DevOps bertanggung jawab untuk mengelola
semua repositori dengan efisien dan keamanan yang tinggi. Manajer tim DevOps mengambil
langkah strategis dengan mengimplementasikan pendekatan DevSecOps pada salah satu
repositori yang dikelola.</p>
<p>Implementasi DevSecOps ini dilakukan dengan memanfaatkan GitLab SAST (Static
Application Security Testing) di dalam CI (Continuous Integration) Pipeline. Dengan menggunakan
GitLab SAST, setiap perubahaan atau push yang dilakukan pada repositori secara otomatis
menjalani proses pemindaian untuk mendeteksi potensi kerentanan (vulnerbilities) dalam kode.</p>
<p>Hal ini memungkinkan tim untuk mengidentifikasi dan mengatasi masalah keamanan sejak
tahap awal dalam siklus pengembangan, sehingga meningkatkan kualitas dan keamanan
perangkat lunak yang dihasilkan. Ini adalah langkah penting dalam menciptakan budaya
keamanan yang berkelanjutan di dalam tim pengembangan perangkat lunak.</p>

## Tools yang digunakan:
- GitLab Community Edition v16.8.1
- GitLab Runner v17.5.2
- SAST
- Docker v27.3.1
- Python Alpine

## Topologi
![git clone](assets/images/git clone.png)

## Alur Kerja
![git clone](assets/images/git clone (1).png)

<p>Nah sebelum ke langkah pengerjaan, kita bahas teori nya dulu yukk!</p>

## Teori 
### 1. DevOps
<p>DevOps merupakan singkatan dari dua kata yaitu “Development” dan “Operations”.
DevOps merupakan gabungan dari budaya, alat dan praktik dalam pengembangan perangkat
lunak yang menggabungkan proses pengembangan dengan proses operasi. DevOps mendorong
kerja sama antara Tim Pengembangan dan Tim Operasi. Dengan DevOps pengembangan
perangkat lunak menjadi lebih efektif.</p>

### 2. DevSecOps
<p>DevSecOps adalah praktik mengintegrasikan pengujian keamanan di setiap tahap proses
pengembangan perangkat lunak. DevSecOps mencakup berbagai alat dan proses yang
mendorong kolaborasi antara pengembangan, spesialis keamanan, dan tim operasi untuk
membangun perangkat lunak yang efisien dan aman.</p>

### 3. GitLab
<p>GitLab adalah repositori Git berbasis web yang menyediakan repositori terbuka dan pribadi
secara gratis, kapabilitas pelacakan masalah, dan wiki. Ini adalah platform DevOps lengkap yang
memungkinkan para profesional untuk melakukan semua tugas dalam sebuah proyek mulai dari
perencanaan proyek dan manajemen kode sumber hingga pemantauan dan keamanan. Selain itu,
platform ini memungkinkan tim untuk berkolaborasi dan membangun perangkat lunak yang lebih
baik.</p>

### 4. GitLab Runner
<p>Gitlab Runner adalah eksekutor yang digunakan untuk menjalankan pekerjaan CI/CD dan
mengirimkan hasilnya kembali ke GitLab. Runner adalah pekerja keras dari pipeline CI/CD GitLab,
yang melakukan tugas-tugas seperti membangun, menguji, dan men-deploy kode. Mereka
fleksibel dan dapat menjalankan pekerjaan di lingkungan yang berbeda, seperti Docker, shell, dan
Kubernetes.</p>

### 5. SAST
<p>GitLab SAST (Static Application Security Testing) adalah fitur di GitLab yang otomatis
memeriksa kode aplikasi untuk menemukan potensi kerentanan atau kelemahan keamanan
sebelum aplikasi dijalankan. Penganalisis mengeluarkan laporan berformat JSON sebagai job
artifacts.</p>

### 6. Pipeline
<p>Pipeline di GitLab adalah kumpulan tahapan dan pekerjaan yang mengotomatisasikan
proses pengembangan perangkat lunak. Pipeline GitLab mengeksekusi beberapa pekerjaan,
tahap demi tahap, dengan bantuan kode otomatis.</p>

### 7. Container
<p>Container adalah unit standar perangkat lunak yang mengemas kode dan semua
depedensi nya sehingga aplikasi berjalan dengan cepat, serta dapat diandalkan dari satu
lingkungan komputasi ke lingkungan komputasi lainnya. Beberapa container dapat berjalan pada
mesin yang sama dan berbagi kernel OS dengan container lain, masing-masing berjalan sebagai
proses yang terisolasi dalam ruang pengguna.</p>

### 8. Docker
<p>Docker adalah layanan yang menyediakan kemampuan untuk mengemas dan menjalankan
sebuah aplikasi dalam sebuah lingkungan terisolasi yang disebut dengan container. Dengan
adanya isolasi dan keamanan yang memadai memungkinkan untuk menjalankan banyak container
di waktu yang bersamaan.</p>

### 9. Python Flask
<p>Python Flask adalah kerangka kerja (framework) web ringan dan sederhana yang ditulis
dalam bahasa pemrograman Python. Flask dianggap ringan karena tidak menuntut banyak hal dari
sistem atau memerlukan banyak depedensi dibandingkan kerangka kerja Python lainnya.</p>

## Langkah Implementasi
