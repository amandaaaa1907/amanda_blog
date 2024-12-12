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
  <img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/git-clone.png" alt="Topologi">

## Alur Kerja
  <img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/alur-kerja.png" alt="Topologi">

<p>Nah, sebelum ke langkah pengerjaan, kita bahas teori nya dulu yukk!</p>

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
### Install Docker
```
# Add Docker's official GPG key:
$ sudo apt-get update
$ sudo apt-get install ca-certificates curl
$ sudo install -m 0755 -d /etc/apt/keyrings
$ sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
$ sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
### Install GitLab Runner
1). Install Gitlab Runner lalu tambahkan mode execute
```
$ sudo curl -L --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64
$ sudo chmod +x /usr/local/bin/gitlab-runner
```
2). Buat pengguna untuk Gitlab CI
```
$ sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
```
3). Install dan jalankan service Gitlab Runner
```
$ sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
$ sudo gitlab-runner start
```
4). Tambahkan pengguna Gitlab Runner ke grup docker
```
$ sudo usermod -aG docker gitlab-runner
$ sudo -u gitlab-runner -H docker info
```
### 3. Membuat GitLab Akun dan Project
<p>1. Buat akun Gitlab terlebih dahulu, login, sign in, atau bisa juga register</p>
<p>2. Untuk membuat project baru pilih Projects > New Project lalu klik "Create blank project"</p>
<p>3. Isi nama project, pilih Visibility Level Public, lalu klik "Create project"</p>

### 4. Clone Repository dari Github dan Konfigurasi GitLab
1). Clone repository dari github [Link Repo](https://github.com/bta-adinusa/flask-todo-app).
<p>2). Masuk ke dalam direktori dari repository tersebut</p>
3). Buat konfigurasi global untuk menyediakan informasi tentang nama dan email

```
$ git config --global user.name "masukkan username"
$ git config --global user.mail "masukkan email"
$ git config --global init.defaultBranch main
$ git config --global --list
```

4). Ubah URL repository menjadi URL ke repository Gitlab, setelah itu verifikasi hasil set url

```
$ git remote set-url origin https://gitlab.com/username/repo.git
$ git remote -v
```
### 5. Membuat Dockerfile
<div class="note">
    <strong>Note:</strong> Karena mendeploy web flask-todo-app nya meggunakan docker jadi buat terlebih dahulu Dockerfile nya.
</div>
<p>1. Buat Dockerfile nya</p>

```
$ sudo nano Dockerfile
```
<p>2. Isi Dockerfile sesuai dengan kebutuhan yang diperlukan, seperti contoh dibawah ini</p>

```
FROM python:alpine
RUN mkdir /app
RUN apk update

COPY . /app

WORKDIR /app

RUN python3 -m venv /app/venv
RUN /app/venv/bin/pip
RUN pip install -r requirements.txt

ENTRYPOINT ["python"]
CMD ["app.py"]
```
>*Note: Karena menggunakan Alpine Linux, gunakan perintah apk update untuk memperbarui package list.*

### 6. Membuat file .gitlab-ci.yml dan SAST.gitlab-ci.yml
<p>1. Buat file untuk gitlab ci</p>

```
$ sudo nano .gitlab-ci.yml
```
<p>2. Isi file gitlab-ci nya sesuai dengan kebutuhan, seperti contoh dibawah ini.</p>

```
include:
  - template: Security/SAST.gitlab-ci.yml

stages:
  - sast
  - deploy

sast:
  stage: sast
  script:
    - echo "Running SAST"

deploy:
  stage:
  image: <image docker>
  needs:
    - job: sast
      artifacts: true
  script:
    - docker build -t (nama untuk image) .
    - docker run -d -p 5000:5000 --name (nama container) (nama image)
    - docker images
    - docker ps
    - sleep 30
    - docker ps
  only:
    - main
```
<p>3. Buat file untuk konfigurasi SAST</p>

```
$ sudo nano SAST.gitlab-ci.yml
```
4.Karena menggunakan template untuk konfigurasi SAST nya, teman-teman bisa lihat [disini](https://gitlab.com/gitlab-org/gitlab/-/blob/master/lib/gitlab/ci/templates/Jobs/SAST.gitlab-ci.yml).

### 7. Membuat GitLab Runner dan Push Kode
<p>1. Sebelum membuat runner, ambil token terlebih dahulu di bagian Settings > CI/CD > </p>
<p>2. Klik Expand pada bagian Runners</p>
<p>3. Klik titik 3 pada Project runners, lalu salin token yang ada</p>
<p>4. Register Gitlab Runner dengan menggunakan executor docker</p>

```
$ sudo gitlab-runner register -n \
  --url https://gitlab.com \
  --registration-token (token) \
  --executor docker \
  --description "Docker Runner" \
  --docker-image ("image docker") \
  --docker-volume /var/run/docker.sock:/var/run/docker.sock
```

<p>5. Setelah Runner sudah terbuat, push kode ke gitlab</p>

```
$ git add .
$ git commit -m "commant" && git push origin main
```

### 8. Mendownload Artifacts File JSON
1). Untuk mendownload hasil dari artifacts bisa di download di bagian Build > Artifacts. Download file yang format JSON.

2). Tampilan JSON yang sudah di download akan berantakan dan jadi tidak terstruktur, teman-teman bisa gunakan web untuk memformat ulang tampilan JSON agar jadi lebih terstruktur. [Referensi website](https://jsonlint.com/)

### 9. Membuat Alert Untuk Pipeline dan Vulnerability
*Note: Alert nya disini menggunakan Discord ya*

1). Buka Discord channel tempat yang ingin dipakai untuk menerima notifikasi, lalu pilih **Edit CHannel**

<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/Screenshot 2024-11-11-110932.png" alt="Discord">

2). Pilih bagian **Integrations**
<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/Screenshot 2024-11-11.png" alt="Discord">

3). Jika belum ada webhook, pilih bagian **Create Webhook**, lalu pilih **View Webhook**, setelah itu klik **New Webhook**. Kalau webhook sudah ada salin URL dari kolom **Copy Webhook URL**

<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/Screenshot 2024-11-112.png" alt="Discord">
