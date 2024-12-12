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
---

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

---

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
---

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
<p>1. Install Gitlab Runner lalu tambahkan mode execute.</p>

```
$ sudo curl -L --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64
$ sudo chmod +x /usr/local/bin/gitlab-runner
```
<p>2. Buat pengguna untuk Gitlab CI.</p>

```
$ sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
```
<p>3. Install dan jalankan service Gitlab Runner</p>

```
$ sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
$ sudo gitlab-runner start
```
<p>4. Tambahkan pengguna Gitlab Runner ke grup docker.</p>

```
$ sudo usermod -aG docker gitlab-runner
$ sudo -u gitlab-runner -H docker info
```
### Membuat GitLab Akun dan Project
<p>1. Buat akun Gitlab terlebih dahulu, login, sign in, atau bisa juga register.</p>
<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/ss9.png" alt="Gitlab">

<p>2. Untuk membuat project baru pilih Projects > New Project lalu klik "Create blank project".</p>
<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/ss10.png" alt="Gitlab">
<p>3. Isi nama project, pilih Visibility Level Public, lalu klik "Create project".</p>
<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/ss11.png" alt="Gitlab">

### Clone Repository dari Github dan Konfigurasi GitLab
<p>1. Clone repository dari github <a href="https://github.com/bta-adinusa/flask-todo-app">Link Repo</a>.</p>

<p>2. Masuk ke dalam direktori dari repository tersebut.</p>
<p>3. Buat konfigurasi global untuk menyediakan informasi tentang nama dan email.</p>

```
$ git config --global user.name "masukkan username"
$ git config --global user.mail "masukkan email"
$ git config --global init.defaultBranch main
$ git config --global --list
```

<p>4. Ubah URL repository menjadi URL ke repository Gitlab, setelah itu verifikasi hasil set url.</p>

```
$ git remote set-url origin https://gitlab.com/username/repo.git
$ git remote -v
```
### Membuat Dockerfile
*Note: Karena mendeploy web flask-todo-app nya meggunakan docker jadi buat terlebih dahulu Dockerfile nya.*
<p>1. Buat Dockerfile nya.</p>

```
$ sudo nano Dockerfile
```
<p>2. Isi Dockerfile sesuai dengan kebutuhan yang diperlukan, seperti contoh dibawah ini.</p>

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
*Note: Karena menggunakan Alpine Linux, gunakan perintah apk update untuk memperbarui package list.*

### Membuat file .gitlab-ci.yml dan SAST.gitlab-ci.yml
<p>1. Buat file untuk gitlab ci.</p>

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
  image: docker:latest # gunakan image docker
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
<p>3. Buat file untuk konfigurasi SAST.</p>

```
$ sudo nano SAST.gitlab-ci.yml
```
<p>4. Karena menggunakan template untuk konfigurasi SAST nya, teman-teman bisa gunakan template yang sudah di sediakan Gitlab.</p>

### Membuat GitLab Runner dan Push Kode
<p>1. Sebelum membuat runner, ambil token terlebih dahulu di bagian Settings > CI/CD.</p>
<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/ss12.png" alt="Gitlab">
<p>2. Klik Expand pada bagian Runners</p>
<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/ss13.png" alt="Gitlab">
<p>3. Klik titik 3 pada Project runners, lalu salin token yang ada</p>
<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/ss14.png" alt="Gitlab">
<p>4. Register Gitlab Runner dengan menggunakan executor docker</p>

```
$ sudo gitlab-runner register -n \
  --url https://gitlab.com \
  --registration-token (token) \
  --executor docker \
  --description "Docker Runner" \
  --docker-image "docker:latest" \ 
  --docker-volume /var/run/docker.sock:/var/run/docker.sock
```

<p>5. Setelah Runner sudah terbuat, push kode ke gitlab</p>

```
$ git add .
$ git commit -m "commant" && git push origin main
```

### Mendownload Artifacts File JSON
<p>1. Untuk mendownload hasil dari artifacts bisa di download di bagian Build > Artifacts. Download file yang format JSON.</p>

<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/ss15.png" alt="Gitlab">

<p>2. Tampilan JSON yang sudah di download akan berantakan dan jadi tidak terstruktur, teman-teman bisa gunakan web untuk memformat ulang tampilan JSON agar jadi lebih terstruktur.</p>

### Membuat Alert Discord Untuk Pipeline dan Vulnerability
*Note: Alert nya disini menggunakan Discord ya*

<p>1. Buka Discord channel tempat yang ingin dipakai untuk menerima notifikasi, lalu pilih <b>Edit Channel.</b></p>

<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/Screenshot 2024-11-11-110932.png" alt="Discord">

<p>2. Pilih bagian <b>Integrations.</b></p>
<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/Screenshot 2024-11-11.png" alt="Discord">

<p>3. Jika belum ada webhook, pilih bagian <b>Create Webhook</b>, lalu pilih <b>View Webhook</b>, setelah itu klik <b>New Webhook</b>. Kalau webhook sudah ada salin URL dari kolom <b>Copy Webhook URL</b>.</p>

<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/Screenshot 2024-11-112.png" alt="Discord">
<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/Screenshot 2024-11-113.png" alt="Discord">

<p>4. Beralih ke Gitlab, di bagian <b>Settings</b> pilih Integrations.</p>

<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/ss1.png" alt="Discord">

<p>5. Pilih <b>Discord Notification</b>, lalu pilih Configure.</p>

<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/ss2.png" alt="Discord">

*Note: Karena saya sudah pernah buat jadi sudah active.*

<p>6. Di tahap ini masukkan URL webhook yang telah di salin sebelumnya, lalu pilih kotak yang ingin di centang sesuai dengan yang ingin dikirim notifikasi ke Discord. Setelah itu pilih <b>Save changes</b>.</p>

<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/ss3.png" alt="Discord">

<p>7. Contoh tampilan notifikasi Discord.</p>
<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/ss4.png" alt="Discord">

*Note: untuk alerting vulnerability menggunakan telegram*

<p>8. Tambahkam job untuk alert telegram di dalam file <i>.gitlab-ci.yml</i>.</p>

```
notify-telegram:
  stage: notify
  image: python:3.9-alpine
  before_script:
    - pip install requests
  script:
    - echo "Sending SAST results to Telegram..."
    - if [ ! -f gl-sast-report,json ]; then echo "Artifact not found!" && exit 1; fi
    - python notify-telegram.py gl-sast-report.json
```
<p>9. Edit file <i>notify-telegram.py</i> untuk membuat script alert.</p>

```
$ sudo nano notify-telegram.py

# isi dari file notify-telegram.py
import json
import requests
import sys
import os

# Telegram Bot Token dan Chat ID
TELEGRAM_TOKEN = os.getenv("TELEGRAM_TOKEN")  # TELEGRAM_TOKEN ada di variables gitlab
CHAT_ID = os.getenv("CHAT_ID")  # CHAT_ID ada di variables gitlab

def send_telegram_message(message):
    url = f"https://api.telegram.org/bot{TELEGRAM_TOKEN}/sendMessage"
    payload = {
        "chat_id": CHAT_ID,
        "text": message,
        "parse_mode": "HTML"
    }
    response = requests.post(url, json=payload, timeout=10)
    if response.status_code == 200:
        print("Message sent successfully!")
    else:
        print(f"Failed to send message: {response.status_code}, {response.text}")

def parse_sast_report(json_file):
    with open(json_file, "r") as f:
        data = json.load(f)

    vulnerabilities = data.get("vulnerabilities", [])
    if not vulnerabilities:
        return "✅ <b>No vulnerabilities found</b> in the SAST report."

    # Buat ringkasan hasil
    message = "⚠️ <b>SAST Vulnerabilities Found:</b>\n\n"
    for vuln in vulnerabilities[:5]:  # Kirim maksimal 5 issue
        message += (
            f"🔹 <b>ID:</b> {vuln.get('id', 'N/A')}\n"
            f"    <b>Severity:</b> {vuln.get('severity', 'N/A')}\n"
            f"    <b>Message:</b> {vuln.get('message', 'N/A')}\n"
            f"    <b>File:</b> {vuln.get('location', {}).get('file', 'N/A')} (line {vuln.get('location', {}).get('start_line', 'N/A')})\n\n"
        )
    message += "\n🚨 Check details in the full SAST report."

    return message

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print("Usage: python notify_telegram.py <sast_json>")
        sys.exit(1)

    json_file = sys.argv[1]
    try:
        sast_message = parse_sast_report(json_file)
        send_telegram_message(sast_message)
    except Exception as e:
        print(f"Error: {e}")
        sys.exit(1)
```
<p>10. Buat bot di telegram, bot ini untuk mengirimkan alert dari vulnerability.</p>

<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/ss5.png" alt="Telegram">

*Note: karena saya sudah pernah buat bot sebelumnya, jadi saya rename untuk nama botnya. Setelah bot dibuat salin tokennya.*

<p>11. Akses web untuk mendapatkan informasi bot.</p>

```
https://api.telegram.org/bot(token)/getMe
```
<p>12. Coba kirim pesan ke bot, setelah itu cek di browser apakah sudah mendapatkan chat id.</p>

<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/ss6.png" alt="Telegram">

```
https://api.telegram.org/bot(token)/getUpdates
```
<p>13. Setelah mendapatkan chat id, masukkan ke dalam variables di gitlab CI/CD. Ke Settings > CI/CD > Variables. Isi variables dengan CHAT_ID dan TELEGRAM_TOKEN.</p>

<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/ss7.png" alt="Telegram">

<img src="https://amandaaaa1907.github.io/amanda_blog//assets/images/ss8.png" alt="Telegram">

--- 
## Referensi
<a href="https://gitlab.com/gitlab-org/gitlab/-/blob/master/lib/gitlab/ci/templates/Jobs/SAST.gitlab-ci.yml">Template SAST</a>

<a href="https://youtu.be/owwIMUamdDc?si=0qhySUZ5FBd9Lrid">GitLab: DevSecOps: Part 5/12: Protect your Apps with Static Application Security Testing (SAST)</a>

<a href="https://youtu.be/jfP11CHsz3E?si=yL9Vbc1Ycf3xkHZY">Build full CICD Pipelines for Docker Flask App in GitLab</a>

<a href="https://jsonlint.com/">Referensi Website untuk memformat ulang file JSON</a>

<a href="https://web.telegram.org/k/#@BotFather">Gunakan BotFather untuk membuar bot</a>

<a href="https://github.com/bta-adinusa/flask-todo-app">Link Repository</a>

---

<p>Nah jadi gitu teman-teman prosesnya, gimana? cukup rumit dan bikin pusing? Dengan ini kita dapat mendeteksi kerentanan sejak dini! Semoga panduan ini memberikan wawasan dan bermanfaat dalam mengimplementasikan GitLab SAST.</p>
<p>Terima kasih telah membaca, dan sampai jumpa di artikel lainnya!👋</p>

<div class="breaker"></div>