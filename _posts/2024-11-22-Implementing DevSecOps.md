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

## Alur Kerja

<p>Nah sebelum ke langkah pengerjaan, kita bahas teori nya dulu yukk!</p>

## Teori 
#### 1. DevOps
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

