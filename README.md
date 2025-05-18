package main

import "fmt"

type Pakaian struct {
	Nama       string
	Kategori   string
	Warna      string
	Formalitas int
}

var daftarPakaian []Pakaian
var jumlahPakaian int

func tambahPakaian(nama, kategori, warna string, formalitas int) {
	if jumlahPakaian == len(daftarPakaian) {
		temp := make([]Pakaian, jumlahPakaian+1)
		for i := 0; i < jumlahPakaian; i++ {
			temp[i] = daftarPakaian[i]
		}
		daftarPakaian = temp
	}
	daftarPakaian[jumlahPakaian] = Pakaian{Nama: nama, Kategori: kategori, Warna: warna, Formalitas: formalitas}
	jumlahPakaian++
	fmt.Println("Pakaian berhasil ditambahkan!")
}

func hapusPakaian(nama string) {
	for i := 0; i < jumlahPakaian; i++ {
		if daftarPakaian[i].Nama == nama {
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

func buatKombinasiOOTD() {
	fmt.Println("Kombinasi Outfit:")
	for i := 0; i < jumlahPakaian; i++ {
		pakaian := daftarPakaian[i]
		fmt.Printf("- %s (%s, %s, Formalitas: %d)\n", pakaian.Nama, pakaian.Kategori, pakaian.Warna, pakaian.Formalitas)
	}
}

func cariPakaianBinary(kriteria, nilai string) {
	// Bubble Sort untuk mengurutkan daftarPakaian berdasarkan kriteria
	for i := 0; i < jumlahPakaian-1; i++ {
		for j := 0; j < jumlahPakaian-i-1; j++ {
			if (kriteria == "warna" && daftarPakaian[j].Warna > daftarPakaian[j+1].Warna) ||
				(kriteria == "kategori" && daftarPakaian[j].Kategori > daftarPakaian[j+1].Kategori) {
				// Tukar elemen
				temp := daftarPakaian[j]
				daftarPakaian[j] = daftarPakaian[j+1]
				daftarPakaian[j+1] = temp
			}
		}
	}

	// Binary Search
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

func rekomendasiOutfit(acara string) {
	fmt.Println("Rekomendasi outfit:")
	switch acara {
	case "hujan":
		for i := 0; i < jumlahPakaian; i++ {
			pakaian := daftarPakaian[i]
			if containsSubstring(pakaian.Nama, "jaket") || containsSubstring(pakaian.Nama, "mantel") {
				fmt.Printf("- %s (%s, %s, Formalitas: %d)\n", pakaian.Nama, pakaian.Kategori, pakaian.Warna, pakaian.Formalitas)
			}
		}
	case "formal":
		for i := 0; i < jumlahPakaian; i++ {
			pakaian := daftarPakaian[i]
			if pakaian.Formalitas == 3 {
				fmt.Printf("- %s (%s, %s, Formalitas: %d)\n", pakaian.Nama, pakaian.Kategori, pakaian.Warna, pakaian.Formalitas)
			}
		}
	default:
		fmt.Println("Tidak ada rekomendasi untuk acara tersebut.")
	}
}

// Fungsi tambahan untuk memeriksa substring (case-sensitive)
func containsSubstring(s, substr string) bool {
	n := len(s)
	m := len(substr)
	if m > n {
		return false
	}
	for i := 0; i <= n-m; i++ {
		if s[i:i+m] == substr {
			return true
		}
	}
	return false
}

func main() {
	for {
		fmt.Println("\nMenu:")
		fmt.Println("1. Tambah Pakaian")
		fmt.Println("2. Ubah Pakaian")
		fmt.Println("3. Hapus Pakaian")
		fmt.Println("4. Cari Pakaian (Binary)")
		fmt.Println("5. Buat Kombinasi OOTD")
		fmt.Println("6. Rekomendasi Outfit")
		fmt.Println("7. Tampilkan Pakaian")
		fmt.Println("8. Keluar")
		fmt.Print("Pilih menu: ")

		var pilihan int
		fmt.Scan(&pilihan)

		switch pilihan {
		case 1:
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
			var nama string
			fmt.Print("Nama pakaian yang ingin dihapus: ")
			fmt.Scan(&nama)
			hapusPakaian(nama)
		case 4:
			var kriteria, nilai string
			fmt.Print("Cari berdasarkan (warna/kategori): ")
			fmt.Scan(&kriteria)
			fmt.Print("Nilai: ")
			fmt.Scan(&nilai)
			cariPakaianBinary(kriteria, nilai)
		case 5:
			buatKombinasiOOTD()
		case 6:
			var acara string
			fmt.Print("Acara (hujan/formal): ")
			fmt.Scan(&acara)
			rekomendasiOutfit(acara)
		case 7:
			tampilkanPakaian()
		case 8:
			fmt.Println("Keluar dari aplikasi.")
			return
		default:
			fmt.Println("Pilihan tidak valid.")
		}
	}
}
