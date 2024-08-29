# Pengelolaan-Memori-pada-Stack-vs-Heap
Berikut adalah penyusunan laporan berdasarkan eksperimen tentang Pengelolaan Memori pada Stack vs Heap:

---

### **1. Tujuan Eksperimen**
Tujuan dari eksperimen ini adalah untuk memahami perbedaan pengelolaan memori antara stack dan heap, menentukan kapan masing-masing lebih efektif digunakan, serta mengidentifikasi risiko seperti stack overflow dan memory leak yang terkait dengan penggunaan berlebihan dari masing-masing area memori.

### **2. Latar Belakang**
Memori pada komputer dikelola di dua area utama, yaitu stack dan heap. Stack digunakan untuk menyimpan variabel lokal dan data rekursif dalam fungsi, sementara heap digunakan untuk alokasi memori dinamis yang dapat bertahan lebih lama dari lingkup fungsi. Pengelolaan memori yang buruk di kedua area ini dapat menyebabkan masalah seperti stack overflow dan memory leak, yang berpotensi menurunkan kinerja program atau menyebabkan kegagalan aplikasi.

### **3. Deskripsi Problem**
- **Masalah di Stack:** Stack memiliki ukuran yang terbatas dan dikelola oleh pola Last-In-First-Out (LIFO). Rekursi berlebih atau alokasi variabel besar pada stack dapat menyebabkan stack overflow.
- **Masalah di Heap:** Heap memungkinkan alokasi memori yang lebih fleksibel tetapi rentan terhadap memory leak jika memori yang dialokasikan tidak dibebaskan setelah digunakan. Heap juga dapat mengalami fragmentasi memori yang mengurangi efisiensi alokasinya.

### **4. Metodologi Eksperimen**
Eksperimen ini membandingkan penggunaan memori stack dan heap dengan cara:
1. Membuat program yang memanfaatkan stack secara intensif menggunakan rekursi dan variabel lokal besar.
2. Membuat program yang memanfaatkan heap secara intensif menggunakan alokasi dinamis dengan `malloc` dan `free`.
3. Mengukur kinerja dan mencatat hasil dari setiap skenario untuk melihat kapan stack lebih efisien dibanding heap, dan sebaliknya.

### **5. Pelaksanaan Eksperimen (Source Code)**

#### **Program 1: Penggunaan Memori pada Stack**
Program ini menggunakan rekursi untuk mengalokasikan banyak data di stack, yang bisa memicu stack overflow jika terlalu dalam.

```c
#include <stdio.h>

void stackIntensiveFunction(int n) {
    // Buat array besar untuk memanfaatkan stack
    int stackArray[1000];

    // Isi array dengan nilai untuk menggunakan memori
    for (int i = 0; i < 1000; i++) {
        stackArray[i] = i;
    }

    // Rekursi sampai mencapai batas atau stack overflow
    if (n > 0) {
        stackIntensiveFunction(n - 1);
    }
}

int main() {
    // Panggil fungsi rekursif dengan jumlah besar
    stackIntensiveFunction(10000);  // Ubah angka ini untuk uji coba lebih dalam
    printf("Program selesai menggunakan stack.\n");
    return 0;
}
```

#### **Program 2: Penggunaan Memori pada Heap**
Program ini menggunakan alokasi dinamis dengan `malloc` dan membebaskan memori dengan `free`. Ini mengukur kinerja alokasi memori heap dan memeriksa adanya potensi memory leak.

```c
#include <stdio.h>
#include <stdlib.h>

void heapIntensiveFunction(int n) {
    // Alokasikan memori pada heap
    int *heapArray = (int*)malloc(1000 * sizeof(int));

    // Cek apakah alokasi berhasil
    if (heapArray == NULL) {
        printf("Gagal mengalokasikan memori pada heap!\n");
        return;
    }

    // Isi array dengan nilai untuk menggunakan memori
    for (int i = 0; i < 1000; i++) {
        heapArray[i] = i;
    }

    // Bebaskan memori untuk mencegah memory leak
    free(heapArray);

    // Rekursi jika masih ada iterasi yang tersisa
    if (n > 0) {
        heapIntensiveFunction(n - 1);
    }
}

int main() {
    // Panggil fungsi dengan alokasi memori yang intensif
    heapIntensiveFunction(10000);  // Ubah angka ini untuk uji coba lebih dalam
    printf("Program selesai menggunakan heap.\n");
    return 0;
}
```

### **6. Hasil Eksperimen**
- **Stack Program:** Pada iterasi besar, stack program mulai mengalami stack overflow ketika jumlah panggilan rekursif melebihi batas kapasitas stack. Ini menunjukkan batasan penggunaan stack untuk alokasi memori yang besar atau rekursi dalam.
- **Heap Program:** Heap program berhasil mengalokasikan memori dengan lebih fleksibel, tetapi menunjukkan risiko memory leak jika alokasi tidak dibebaskan. Tidak ada stack overflow terjadi, tetapi heap dapat habis jika ada kebocoran memori atau alokasi yang berlebihan.

### **7. Kesimpulan**
- **Stack lebih baik digunakan** untuk variabel lokal dan alokasi memori yang hidup singkat seperti variabel fungsi dan rekursi yang terkontrol. Stack menawarkan kecepatan dan efisiensi alokasi karena sifat LIFO, tetapi terbatas oleh ukuran tetapnya dan berisiko mengalami stack overflow dengan penggunaan berlebihan.
- **Heap lebih baik digunakan** untuk alokasi memori besar, dinamis, dan jangka panjang. Heap menawarkan fleksibilitas yang lebih besar, tetapi memerlukan pengelolaan yang hati-hati untuk mencegah memory leak dan fragmentasi.
- **Risiko Stack:** Stack overflow jika penggunaan memori berlebihan.
- **Risiko Heap:** Memory leak jika alokasi tidak dibebaskan dengan benar, dan fragmentasi yang dapat memengaruhi kinerja alokasi memori di masa depan.

Pemahaman yang baik tentang kapan dan bagaimana menggunakan stack dan heap adalah kunci untuk pengelolaan memori yang efisien dan penghindaran kesalahan umum seperti stack overflow dan memory leak.
