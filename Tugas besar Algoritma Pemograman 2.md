package main

import "fmt"

// Struct untuk menyimpan data pakaian
type Pakaian struct {
	Nama       string
	Kategori   string
	Warna      string
	Formalitas int // 1 = kasual, 2 = semi-formal, 3 = formal
}

var daftarPakaian []Pakaian  // Slice untuk menyimpan data pakaian
var jumlahPakaian int        // Jumlah pakaian yang saat ini tersimpan

// Fungsi untuk menambahkan pakaian ke daftar
func tambahPakaian(nama, kategori, warna string, formalitas int) {
	// Jika kapasitas slice penuh, buat slice baru yang lebih besar
	if jumlahPakaian == len(daftarPakaian) {
		temp := make([]Pakaian, jumlahPakaian+1)
		for i := 0; i < jumlahPakaian; i++ {
			temp[i] = daftarPakaian[i]
		}
		daftarPakaian = temp
	}
	// Tambahkan pakaian baru ke slice
	daftarPakaian[jumlahPakaian] = Pakaian{Nama: nama, Kategori: kategori, Warna: warna, Formalitas: formalitas}
	jumlahPakaian++
	fmt.Println("Pakaian berhasil ditambahkan!")
}

// Fungsi untuk menghapus pakaian berdasarkan nama
func hapusPakaian(nama string) {
	for i := 0; i < jumlahPakaian; i++ {
		if daftarPakaian[i].Nama == nama {
			// Geser elemen setelahnya ke kiri
			for j := i; j < jumlahPakaian-1; j++ {
				daftarPakaian[j] = daftarPakaian[j+1]
			}
			jumlahPakaian--
			fmt.Println("Pakaian berhasil dihapus!")
			return
		}
	}
	fmt.Println("Pakaian tidak ditemukan!")
}

// Fungsi untuk mengubah informasi pakaian berdasarkan nama
func ubahPakaian(nama string, kategori, warna string, formalitas int) {
	for i := 0; i < jumlahPakaian; i++ {
		if daftarPakaian[i].Nama == nama {
			daftarPakaian[i] = Pakaian{Nama: nama, Kategori: kategori, Warna: warna, Formalitas: formalitas}
			fmt.Println("Pakaian berhasil diubah!")
			return
		}
	}
	fmt.Println("Pakaian tidak ditemukan!")
}

// Fungsi untuk menampilkan semua pakaian
func tampilkanPakaian() {
	if jumlahPakaian == 0 {
		fmt.Println("Belum ada pakaian yang ditambahkan.")
		return
	}

	fmt.Println("Daftar Pakaian:")
	for i := 0; i < jumlahPakaian; i++ {
		pakaian := daftarPakaian[i]
		fmt.Printf("%d. %s (%s, %s, Formalitas: %d)\n", i+1, pakaian.Nama, pakaian.Kategori, pakaian.Warna, pakaian.Formalitas)
	}
}

// Fungsi untuk membuat kombinasi outfit berdasarkan acara
func buatKombinasiOOTD(acara string) {
	var atasan, bawahan, luaran Pakaian
	var foundAtasan, foundBawahan, foundLuaran bool

	// Pilih atasan berdasarkan formalitas
	fmt.Println("Pilih atasan:")
	for i, p := range daftarPakaian {
		if p.Kategori == "Atasan" &&
			((acara == "informal" && p.Formalitas <= 2) || (acara == "formal" && p.Formalitas == 3)) {
			fmt.Printf("%d. %s (%s, Formalitas: %d)\n", i, p.Nama, p.Warna, p.Formalitas)
		}
	}
	fmt.Print("Masukkan nomor atasan: ")
	var iAtasan int
	fmt.Scan(&iAtasan)
	if iAtasan >= 0 && iAtasan < jumlahPakaian &&
		daftarPakaian[iAtasan].Kategori == "Atasan" &&
		((acara == "informal" && daftarPakaian[iAtasan].Formalitas <= 2) || (acara == "formal" && daftarPakaian[iAtasan].Formalitas == 3)) {
		atasan = daftarPakaian[iAtasan]
		foundAtasan = true
	}

	// Pilih bawahan
	fmt.Println("Pilih bawahan:")
	for i, p := range daftarPakaian {
		if p.Kategori == "Bawahan" &&
			((acara == "informal" && p.Formalitas <= 2) || (acara == "formal" && p.Formalitas == 3)) {
			fmt.Printf("%d. %s (%s, Formalitas: %d)\n", i, p.Nama, p.Warna, p.Formalitas)
		}
	}
	fmt.Print("Masukkan nomor bawahan: ")
	var iBawahan int
	fmt.Scan(&iBawahan)
	if iBawahan >= 0 && iBawahan < jumlahPakaian &&
		daftarPakaian[iBawahan].Kategori == "Bawahan" &&
		((acara == "informal" && daftarPakaian[iBawahan].Formalitas <= 2) || (acara == "formal" && daftarPakaian[iBawahan].Formalitas == 3)) {
		bawahan = daftarPakaian[iBawahan]
		foundBawahan = true
	}

	// Pilih luaran (opsional)
	fmt.Println("Pilih luaran (bisa kosong, tekan -1 jika tidak ingin):")
	for i, p := range daftarPakaian {
		if p.Kategori == "Luaran" &&
			((acara == "informal" && p.Formalitas <= 2) || (acara == "formal" && p.Formalitas == 3)) {
			fmt.Printf("%d. %s (%s, Formalitas: %d)\n", i, p.Nama, p.Warna, p.Formalitas)
		}
	}
	fmt.Print("Masukkan nomor luaran: ")
	var iLuaran int
	fmt.Scan(&iLuaran)
	if iLuaran == -1 {
		foundLuaran = false
	} else if iLuaran >= 0 && iLuaran < jumlahPakaian &&
		daftarPakaian[iLuaran].Kategori == "Luaran" &&
		((acara == "informal" && daftarPakaian[iLuaran].Formalitas <= 2) || (acara == "formal" && daftarPakaian[iLuaran].Formalitas == 3)) {
		luaran = daftarPakaian[iLuaran]
		foundLuaran = true
	}

	// Tampilkan outfit yang dipilih
	fmt.Println("\nOutfit yang kamu pilih:")
	if foundAtasan {
		fmt.Printf("- %s (%s, %s, Formalitas: %d)\n", atasan.Nama, atasan.Kategori, atasan.Warna, atasan.Formalitas)
	}
	if foundBawahan {
		fmt.Printf("- %s (%s, %s, Formalitas: %d)\n", bawahan.Nama, bawahan.Kategori, bawahan.Warna, bawahan.Formalitas)
	}
	if foundLuaran {
		fmt.Printf("- %s (%s, %s, Formalitas: %d)\n", luaran.Nama, luaran.Kategori, luaran.Warna, luaran.Formalitas)
	}
}

// Fungsi untuk mencari pakaian berdasarkan warna atau kategori menggunakan binary search
func cariPakaianBinary(kriteria, nilai string) {
	// Urutkan terlebih dahulu menggunakan insertion sort
	for i := 1; i < jumlahPakaian; i++ {
		key := daftarPakaian[i]
		j := i - 1

		for j >= 0 && ((kriteria == "warna" && daftarPakaian[j].Warna > key.Warna) ||
			(kriteria == "kategori" && daftarPakaian[j].Kategori > key.Kategori)) {
			daftarPakaian[j+1] = daftarPakaian[j]
			j--
		}
		daftarPakaian[j+1] = key
	}

	// Binary search
	low, high := 0, jumlahPakaian-1
	for low <= high {
		mid := (low + high) / 2
		if (kriteria == "warna" && daftarPakaian[mid].Warna == nilai) ||
			(kriteria == "kategori" && daftarPakaian[mid].Kategori == nilai) {
			fmt.Printf("Ditemukan: %s (%s, %s, Formalitas: %d)\n", daftarPakaian[mid].Nama, daftarPakaian[mid].Kategori, daftarPakaian[mid].Warna, daftarPakaian[mid].Formalitas)
			return
		}
		if (kriteria == "warna" && daftarPakaian[mid].Warna < nilai) ||
			(kriteria == "kategori" && daftarPakaian[mid].Kategori < nilai) {
			low = mid + 1
		} else {
			high = mid - 1
		}
	}
	fmt.Println("Pakaian tidak ditemukan!")
}

// Fungsi untuk memberikan rekomendasi outfit sesuai acara
func rekomendasiOutfit(acara string) {
	fmt.Println("Rekomendasi outfit:")
	if acara == "informal" {
		for i := 0; i < jumlahPakaian; i++ {
			if daftarPakaian[i].Formalitas < 3 {
				fmt.Printf("- %s (%s, %s, Formalitas: %d)\n", daftarPakaian[i].Nama, daftarPakaian[i].Kategori, daftarPakaian[i].Warna, daftarPakaian[i].Formalitas)
			}
		}
	} else if acara == "formal" {
		for i := 0; i < jumlahPakaian; i++ {
			if daftarPakaian[i].Formalitas == 3 {
				fmt.Printf("- %s (%s, %s, Formalitas: %d)\n", daftarPakaian[i].Nama, daftarPakaian[i].Kategori, daftarPakaian[i].Warna, daftarPakaian[i].Formalitas)
			}
		}
	} else {
		fmt.Println("Tidak ada rekomendasi untuk acara tersebut.")
	}
}

// Fungsi utama (main) sebagai titik masuk program
func main() {
	// Inisialisasi pakaian awal
	daftarPakaian = []Pakaian{
		{"Kemeja Putih", "Atasan", "Putih", 3},
		{"Celana Jeans", "Bawahan", "Biru", 1},
		{"Jaket Hoodie", "Luaran", "Hitam", 2},
		{"Blazer Hitam", "Luaran", "Hitam", 3},
		{"Kaos Oblong", "Atasan", "Abu", 1},
		{"Rok Span", "Bawahan", "Hitam", 3},
		{"Mantel Hujan", "Luaran", "Biru", 2},
		{"Sweater Rajut", "Atasan", "Coklat", 2},
	}
	jumlahPakaian = len(daftarPakaian)

	// Menu interaktif
	for {
		fmt.Println()
		fmt.Println("+-------------------------------------------+")
		fmt.Println("|         MENU PENGELOLAAN PAKAIAN          |")
		fmt.Println("+-------------------------------------------+")
		fmt.Println("| 1. Tambah Pakaian                         |")
		fmt.Println("| 2. Ubah Pakaian                           |")
		fmt.Println("| 3. Hapus Pakaian                          |")
		fmt.Println("| 4. Cari Pakaian (Binary)                  |")
		fmt.Println("| 5. Buat Kombinasi OOTD                    |")
		fmt.Println("| 6. Rekomendasi Outfit                     |")
		fmt.Println("| 7. Tampilkan Pakaian                      |")
		fmt.Println("| 8. Keluar                                 |")
		fmt.Println("+-------------------------------------------+")
		fmt.Print("Pilih menu: ")

		var pilihan int
		fmt.Scan(&pilihan)

		switch pilihan {
		case 1:
			// Input dan tambah pakaian
			var nama, kategori, warna string
			var formalitas int
			fmt.Print("Nama: ")
			fmt.Scan(&nama)
			fmt.Print("Kategori: ")
			fmt.Scan(&kategori)
			fmt.Print("Warna: ")
			fmt.Scan(&warna)
			fmt.Print("Formalitas (1-3): ")
			fmt.Scan(&formalitas)
			tambahPakaian(nama, kategori, warna, formalitas)
		case 2:
			// Ubah pakaian
			var nama, kategori, warna string
			var formalitas int
			fmt.Print("Nama pakaian yang ingin diubah: ")
			fmt.Scan(&nama)
			fmt.Print("Kategori baru: ")
			fmt.Scan(&kategori)
			fmt.Print("Warna baru: ")
			fmt.Scan(&warna)
			fmt.Print("Formalitas baru (1-3): ")
			fmt.Scan(&formalitas)
			ubahPakaian(nama, kategori, warna, formalitas)
		case 3:
			// Hapus pakaian
			var nama string
			fmt.Print("Nama pakaian yang ingin dihapus: ")
			fmt.Scan(&nama)
			hapusPakaian(nama)
		case 4:
			// Cari pakaian
			var kriteria, nilai string
			fmt.Print("Cari berdasarkan (warna/kategori): ")
			fmt.Scan(&kriteria)
			fmt.Print("Nilai: ")
			fmt.Scan(&nilai)
			cariPakaianBinary(kriteria, nilai)
		case 5:
			// Buat kombinasi OOTD
			var acara string
			fmt.Print("Acara (informal/formal): ")
			fmt.Scan(&acara)
			buatKombinasiOOTD(acara)
		case 6:
			// Rekomendasi outfit
			var acara string
			fmt.Print("Acara (informal/formal): ")
			fmt.Scan(&acara)
			rekomendasiOutfit(acara)
		case 7:
			// Tampilkan semua pakaian
			tampilkanPakaian()
		case 8:
			// Keluar
			fmt.Println("Keluar dari aplikasi.")
			return
		default:
			fmt.Println("Pilihan tidak valid.")
		}
	}
}
