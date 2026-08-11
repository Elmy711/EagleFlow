🦅 EagleFlow – Cara Penggunaan Lengkap

EagleFlow adalah alat uji ketahanan (stress test) L7 berbasis Python yang mendukung berbagai metode HTTP, multi‑threading, proxy, random parameter, mode Slowloris, dan statistik real‑time.
Hanya untuk tujuan edukasi dan pengujian keamanan yang sah.

---

📦 Persyaratan

· Python 3.6 atau lebih baru
· Modul standar (tidak perlu instalasi tambahan)

---

🚀 Instalasi

Simpan file eagleflow.py lalu beri izin eksekusi (opsional):

```bash
chmod +x eagleflow.py
```

---

🎮 Penggunaan Dasar

```bash
python eagleflow.py -t <URL> [opsi]
```

Contoh paling sederhana:

```bash
python eagleflow.py -t http://example.com --threads 500 --duration 30
```

---

🔧 Daftar Opsi (Argumen CLI)

· -t, --target <URL>
    Target URL (wajib jika tidak menggunakan --config atau --targets). Tidak ada default.
· --targets <file>
    File berisi daftar target (satu per baris). Tidak ada default.
· --config <file>
    File konfigurasi (format JSON atau KEY=VALUE). Tidak ada default.
· --threads <jumlah>
    Jumlah thread yang digunakan. Default: 400.
· --sleep <detik>
    Jeda antar thread saat start (dalam detik, bisa float). Default: 0.0.
· --interval <detik>
    Jeda tetap antar request per thread (dalam detik, bisa float). Default: 0.0.
· --min-delay <detik>
    Jeda minimum acak antar request (detik). Jika diatur bersama --max-delay, maka interval menjadi acak. Default: tidak diatur.
· --max-delay <detik>
    Jeda maksimum acak antar request (detik). Default: tidak diatur.
· --duration <detik>
    Durasi serangan dalam detik. Nilai 0 berarti tidak terbatas. Default: 0.
· --timeout <detik>
    Timeout koneksi per request (dalam detik). Default: 10.0.
· -m, --method <METODE>
    Metode HTTP yang digunakan: GET, POST, HEAD, DELETE. Default: GET.
· --data <string>
    Data yang dikirim untuk metode POST. Jika tidak diberikan dan method POST, akan digunakan data bawaan: name=test&email=test@example.com&message=Hello+from+EagleFlow.
· --agent <user-agent>
    User‑Agent kustom (menonaktifkan pemilihan acak). Default: menggunakan 50 User‑Agent bawaan secara acak.
· --cookie <string>
    Kirim cookie, contoh: "session=abc". Default: tidak ada.
· --referer <string>
    Kirim header Referer. Default: tidak ada.
· --header <"Key: Value">
    Header kustom. Dapat digunakan berulang kali untuk beberapa header. Default: tidak ada.
· --proxy <URL>
    Proxy tunggal, contoh: http://proxy:8080. Default: tidak ada.
· --proxies <file>
    File berisi daftar proxy (satu per baris) untuk rotasi acak. Default: tidak ada.
· --random-params
    Tambahkan parameter acak (?rnd=1234) ke setiap URL. Default: False.
· --slowloris
    Aktifkan mode Slowloris (mengirim header secara perlahan untuk mempertahankan koneksi). Default: False.
· --payload <string>
    Payload yang disisipkan ke data POST. Default: tidak ada.
· --no-verify
    Lewati verifikasi SSL. Default: False.
· --verbose
    Tampilkan informasi detail setiap request (termasuk headers dan URL lengkap). Default: False.
· --debug
    Aktifkan mode debug: simpan log ke file (default eagleflow_debug.log). Default: False.
· --stats
    Tampilkan statistik real‑time setiap 5 detik selama serangan berjalan. Default: False.
· --thread-stats
    (Cadangan) Tampilkan statistik per thread (belum diimplementasikan penuh). Default: False.
· -o, --output <file>
    Simpan log ke file tertentu. Jika tidak ditentukan, dan --debug aktif, log akan disimpan ke eagleflow_debug.log.

---

📝 Contoh Penggunaan (Lengkap)

1. Serangan GET dasar

```bash
python eagleflow.py -t http://example.com --threads 300 --duration 60
```

2. POST dengan data bawaan (built‑in)

```bash
python eagleflow.py -t http://example.com/login -m POST --threads 200 --duration 30
```

Data bawaan: name=test&email=test@example.com&message=Hello+from+EagleFlow

3. POST dengan data kustom

```bash
python eagleflow.py -t http://api.example.com -m POST --data "username=admin&password=123"
```

4. HEAD request

```bash
python eagleflow.py -t http://example.com -m HEAD --timeout 5
```

5. DELETE request

```bash
python eagleflow.py -t http://example.com/resource/1 -m DELETE
```

6. Menggunakan proxy tunggal

```bash
python eagleflow.py -t https://target.com --proxy http://192.168.1.100:8080
```

7. Rotasi proxy dari file

```bash
python eagleflow.py -t http://target.com --proxies proxies.txt --threads 100
```

Contoh proxies.txt:

```
http://proxy1:8080
http://proxy2:8080
socks5://proxy3:1080
```

8. Mode Slowloris

```bash
python eagleflow.py -t http://target.com --slowloris --threads 200 --min-delay 0.5 --max-delay 1.5
```

9. Random parameter + random delay

```bash
python eagleflow.py -t http://target.com --random-params --min-delay 0.05 --max-delay 0.2 --threads 400
```

10. Dengan cookie, header, dan referer

```bash
python eagleflow.py -t http://target.com --cookie "session=abc123" --header "X-API-Key: secret" --referer "https://google.com"
```

11. Multi‑target dari file

```bash
python eagleflow.py --targets targets.txt --threads 300 --interval 0.01
```

Contoh targets.txt:

```
http://site1.com
http://site2.com
https://site3.com
```

12. Menggunakan file konfigurasi JSON

Buat config.json:

```json
{
  "target": "http://example.com",
  "threads": 500,
  "duration": 60,
  "method": "GET",
  "interval": 0.01,
  "timeout": 10,
  "stats": true,
  "random_params": true,
  "headers": {"X-Custom": "test"}
}
```

Jalankan:

```bash
python eagleflow.py --config config.json
```

13. Menggunakan file konfigurasi TXT

Buat config.txt:

```
target=http://example.com
threads=500
duration=60
method=POST
data=username=admin&password=123
stats=true
```

Jalankan:

```bash
python eagleflow.py --config config.txt
```

14. Serangan dengan semua fitur (full‑featured)

```bash
python eagleflow.py -t https://api.example.com -m POST --data "key=value" --threads 500 --sleep 0.005 --interval 0.002 --duration 120 --timeout 8 --agent "CustomAgent/2.0" --cookie "session=xyz" --header "Authorization: Bearer token" --proxy http://proxy:8080 --random-params --payload "test=inject" --verbose --stats --debug -o result.log
```

15. Menampilkan bantuan (help)

```bash
python eagleflow.py -h
```

Atau tanpa argumen:

```bash
python eagleflow.py
```

Akan menampilkan contoh penggunaan singkat.

---

📊 Output dan Statistik

Selama serangan, setiap request menampilkan status dengan warna:

· 2xx → hijau ✅
· 3xx / 4xx → kuning ⚠️
· 5xx → merah ❌

Jika opsi --stats diaktifkan, setiap 5 detik muncul:

```
[Stats] Total: 12500, Success: 12000, Failed: 500, RPS: 250.00, Elapsed: 50.0s
```

Di akhir serangan, ditampilkan ringkasan statistik lengkap:

```
=== Final Statistics ===
Total Requests: 15000
Successful: 14200
Failed: 800
Success Rate: 94.67%
Requests per second: 250.00
Duration: 60.00s

Status Code Distribution:
  200: 14000 (93.3%)
  404: 200 (1.3%)
  500: 800 (5.3%)
```

---

🛑 Menghentikan Serangan

· Otomatis – jika --duration diatur, serangan berhenti setelah durasi habis.
· Manual – tekan CTRL+C kapan saja. Semua thread akan berhenti dalam waktu kurang dari 1 detik.

---

📁 Logging

· Jika --debug atau -o digunakan, log disimpan ke file (default: eagleflow_debug.log).
· Log mencakup timestamp, konfigurasi, statistik akhir, dan 100 request terakhir.

---


📜 Lisensi

MIT License – silakan gunakan dan modifikasi, namun dengan tanggung jawab penuh.

---

