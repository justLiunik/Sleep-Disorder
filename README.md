# Sleep-Disorder

## Deskripsi
Proyek ini menganalisis faktor-faktor yang berkaitan dengan gangguan tidur (Insomnia dan Sleep Apnea) menggunakan Exploratory Data Analysis (EDA), lalu membandingkan tujuh model machine learning untuk memprediksinya.

## Dataset
`Gangguan_Tidur.csv` berisi 374 baris dan 13 kolom: jenis kelamin, umur, pekerjaan, durasi dan kualitas tidur, aktivitas fisik, tingkat stres, kategori BMI, tekanan darah, detak jantung, langkah harian, dan label gangguan tidur (`None`, `Sleep Apnea`, `Insomnia`).

Sebanyak 242 baris merupakan duplikat, sehingga hanya ada 132 baris unik. Hal ini diperhitungkan saat evaluasi model.

## Langkah Analisis
1. **Pembersihan data:** membaca `None` sebagai kelas (bukan nilai kosong), menyatukan kategori BMI `Normal Weight` ke `Normal`, dan memecah tekanan darah menjadi `Systolic` dan `Diastolic`.
2. **EDA:** distribusi fitur, perbandingan fitur per kelas gangguan tidur, dan korelasi antar fitur.
3. **Preprocessing:** one-hot encoding dan MinMax scaling di dalam pipeline. Validasi memakai `StratifiedGroupKFold` 5 fold, sehingga baris kembar tidak berada di data latih dan data uji sekaligus.
4. **Modeling:** Gaussian NB, Multinomial NB, SVM (linear), KNN, Decision Tree, Random Forest, dan XGBoost, dievaluasi dengan akurasi dan macro-F1.
5. **Interpretasi:** feature importance Random Forest dan visualisasi pohon keputusan.

## Hasil

| Model | Akurasi | Macro-F1 |
|---|---|---|
| Multinomial NB | 0.884 ± 0.035 | 0.818 ± 0.097 |
| XGBoost | 0.893 ± 0.013 | 0.807 ± 0.082 |
| Random Forest | 0.886 ± 0.016 | 0.796 ± 0.094 |
| SVM (linear) | 0.878 ± 0.032 | 0.795 ± 0.109 |
| KNN (k=32) | 0.860 ± 0.051 | 0.779 ± 0.114 |
| Decision Tree | 0.847 ± 0.070 | 0.763 ± 0.099 |
| Gaussian NB | 0.825 ± 0.053 | 0.740 ± 0.062 |

*Rata-rata ± standar deviasi dari 5 fold.*

- Faktor yang paling membedakan gangguan tidur adalah **tekanan darah, kategori BMI, durasi tidur, dan umur**.
- Empat model teratas praktis setara: selisih skornya lebih kecil dari variasi antar fold.
- **Keterbatasan:** dataset kecil dan kemungkinan sintetis, dan pekerjaan sangat terkait dengan label. Hasil ini bukan alat diagnosis medis.

## Cara Menjalankan
Buat virtual environment dan pasang dependensi:

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

Di macOS/Linux, ganti baris kedua dengan `source .venv/bin/activate`.

Buka `Gangguan_tidur.ipynb` di VS Code (ekstensi Jupyter) atau di Jupyter, lalu jalankan semua sel dari atas (*Restart & Run All*). `requirements.txt` hanya memasang kernel, jadi untuk memakai Jupyter di browser pasang dulu dengan `pip install notebook`, lalu jalankan `jupyter notebook`. Notebook juga bisa dijalankan dari terminal:

```bash
python -m nbconvert --to notebook --execute --inplace Gangguan_tidur.ipynb
```

Notebook berisi sel pemeriksaan otomatis (`assert`). Jika salah satu gagal, eksekusi berhenti di sel tersebut.

## Tools
- Python 3.12
- pandas, NumPy
- scikit-learn, XGBoost
- Matplotlib, seaborn
