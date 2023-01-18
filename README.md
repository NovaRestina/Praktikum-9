# Praktikum-9

Nama    : Nova Restina Maharani

Kelas   : TI.22.B2

NIM     : 312210525


# Python - Exceptions Handling

Pengertian

Pada python, ada dua fitur yang sangat penting untuk menangani kesalahan tidak terduga di dalam program dan juga menambah kemampuan debugging di dalamnya.

Exception Handling - adalah suatu mekanisme pada python yang bertujuan untuk menangani sebuah error pada program. Error ini nantinya akan menghentikan program yang dijalankan dengan cara yang tidak normal.
Assertion - pemeriksaan kewajaran yang dapat di aktifkan atau nonaktifkan setelah kita menyelesaikan uji pada program.

Assertion Statement

Saat menemukan pernyataan, Python mengevaluasi ekspresi yang menyertainya, yang mana diharapkan semoga benar. Jika ekspresi salah, Python memunculkan pengecualian AssertionError. Syntax untuk assert itu adalah : assert Expression[, Arguments] Jika pernyataan gagal, Python menggunakan ArgumentExpression sebagai argumen untuk AssertionError. AssertionError exceptions dapat ditangkap dan ditangani seperti pengecualian lainnya menggunakan try-except statement, tetapi jika dibiarkan, mereka akan menghentikan program dan menghasilkan backtrace.

# Membuat Menu Contac

![Screenshot (54)](https://user-images.githubusercontent.com/115637858/213206783-fa513959-138b-4c98-9515-79647678da81.png)

Modul csv kita butuhkan untuk membaca dan menulis file CSV.

Lalu modul os kita butuhkan untuk melakukan clear screen.

Kita juga menyiapkan variabel global bernama csv_filename untuk menentukan file CSV yang akan digunakan.

Untuk saat ini, variabel ini belum kita pakai.. karena kita belum membuat fungsi CRUDS.

Selanjutnya coba perhatikan fungsi clear_screen()

def clear_screen():

os.system('cls' if os.name == 'nt' else 'clear')
Fungsi ini akan kita gunakan untuk membersihkan layar. Fungsi ini sebenarnya akan menjalankan perintah cls (jika di Windows) dan clear (jika di Linux dan Unix).

Berikutnya kita membuat fungsi show_menu() yang akan menampilkan daftar menu dan menjalankan fungsi tertentu sesuai dengan menu yang dipilih oleh user.

Tereakhir kita membuat fungsi back_to_menu() untuk kembali ke menu utama.

Aplikasi ini belum bisa dijalankan, karena kita belum membuat fungsi-fungsi yang lain seperti show_contact(), create_contact(), edit_contact(), delete_contact(), dan search_contact().

# Source Code

import csv
import os

csv_filename = 'contacts.csv'


def clear_screen():
    os.system('cls' if os.name == 'nt' else 'clear')


def show_menu():
    clear_screen()
    print("=== APLIKASI KONTAK ===")
    print("[1] Lihat Daftar Kotak")
    print("[2] Buat Kontak Baru")
    print("[3] Edit Kontak")
    print("[4] Hapus Kontak")
    print("[5] Cari Kontak")
    print("[0] Exit")
    print("------------------------")
    selected_menu = input("Pilih menu> ")

    if (selected_menu == "1"):
        show_contact()
    elif (selected_menu == "2"):
        create_contact()
    elif (selected_menu == "3"):
        edit_contact()
    elif (selected_menu == "4"):
        delete_contact()
    elif (selected_menu == "5"):
        search_concat()
    elif (selected_menu == "0"):
        exit()
    else:
        print("Kamu memilih menu yang salah!")
        back_to_menu()


def back_to_menu():
    print("\n")
    input("Tekan Enter untuk kembali...")
    show_menu()


def show_contact():
    clear_screen()
    contacts = []
    with open(csv_filename) as csv_file:
        csv_reader = csv.reader(csv_file, delimiter=",")
        for row in csv_reader:
            contacts.append(row)

    if (len(contacts) > 0):
        labels = contacts.pop(0)
        print(f"{labels[0]} \t {labels[1]} \t\t {labels[2]}")
        print("-" * 34)
        for data in contacts:
            print(f'{data[0]} \t {data[1]} \t {data[2]}')
    else:
        print("Tidak ada data!")
    back_to_menu()


def create_contact():
    clear_screen()
    with open(csv_filename, mode='a') as csv_file:
        fieldnames = ['NO', 'NAMA', 'TELEPON']
        writer = csv.DictWriter(csv_file, fieldnames=fieldnames)

        no = input("No urut: ")
        nama = input("Nama lengkap: ")
        telepon = input("No. Telepon: ")

        writer.writerow({'NO': no, 'NAMA': nama, 'TELEPON': telepon})

    back_to_menu()


def search_concat():
    clear_screen()
    contacts = []

    with open(csv_filename, mode="r") as csv_file:
        csv_reader = csv.DictReader(csv_file)
        for row in csv_reader:
            contacts.append(row)

    no = input("Cari berdasrakan nomer urut> ")

    data_found = []

    # mencari contact
    indeks = 0
    for data in contacts:
        if (data['NO'] == no):
            data_found = contacts[indeks]

        indeks = indeks + 1

    if len(data_found) > 0:
        print("DATA DITEMUKAN: ")
        print(f"Nama: {data_found['NAMA']}")
        print(f"Telepon: {data_found['TELEPON']}")
    else:
        print("Tidak ada data ditemukan")
    back_to_menu()


def edit_contact():
    clear_screen()
    contacts = []

    with open(csv_filename, mode="r") as csv_file:
        csv_reader = csv.DictReader(csv_file)
        for row in csv_reader:
            contacts.append(row)

    print("NO \t NAMA \t\t TELEPON")
    print("-" * 32)

    for data in contacts:
        print(f"{data['NO']} \t {data['NAMA']} \t {data['TELEPON']}")

    print("-----------------------")
    no = input("Pilih nomer kontak> ")
    nama = input("nama baru: ")
    telepon = input("nomer telepon baru: ")

    # mencari contact dan mengubah datanya
    # dengan data yang baru
    indeks = 0
    for data in contacts:
        if (data['NO'] == no):
            contacts[indeks]['NAMA'] = nama
            contacts[indeks]['TELEPON'] = telepon
        indeks = indeks + 1

    # Menulis data baru ke file CSV (tulis ulang)
    with open(csv_filename, mode="w") as csv_file:
        fieldnames = ['NO', 'NAMA', 'TELEPON']
        writer = csv.DictWriter(csv_file, fieldnames=fieldnames)
        writer.writeheader()
        for new_data in contacts:
            writer.writerow({'NO': new_data['NO'], 'NAMA': new_data['NAMA'], 'TELEPON': new_data['TELEPON']})

    back_to_menu()


def delete_contact():
    clear_screen()
    contacts = []

    with open(csv_filename, mode="r") as csv_file:
        csv_reader = csv.DictReader(csv_file)
        for row in csv_reader:
            contacts.append(row)

    print("NO \t NAMA \t\t TELEPON")
    print("-" * 32)

    for data in contacts:
        print(f"{data['NO']} \t {data['NAMA']} \t {data['TELEPON']}")

    print("-----------------------")
    no = input("Hapus nomer> ")

    # mencari contact dan mengubah datanya
    # dengan data yang baru
    indeks = 0
    for data in contacts:
        if (data['NO'] == no):
            contacts.remove(contacts[indeks])
        indeks = indeks + 1

    # Menulis data baru ke file CSV (tulis ulang)
    with open(csv_filename, mode="w") as csv_file:
        fieldnames = ['NO', 'NAMA', 'TELEPON']
        writer = csv.DictWriter(csv_file, fieldnames=fieldnames)
        writer.writeheader()
        for new_data in contacts:
            writer.writerow({'NO': new_data['NO'], 'NAMA': new_data['NAMA'], 'TELEPON': new_data['TELEPON']})

    print("Data sudah terhapus")
    back_to_menu()


if __name__ == "__main__":
    while True:
        show_menu()

# Hasil Output

![Screenshot (45)](https://user-images.githubusercontent.com/115637858/213208265-c7c96ed6-a148-4c34-b154-75f8a5c93e8a.png)

## Buat Kontak

![Screenshot (47)](https://user-images.githubusercontent.com/115637858/213208599-74657560-2595-4d03-a811-ef838f409e32.png)

![Screenshot (48)](https://user-images.githubusercontent.com/115637858/213208865-5104aa71-776f-47ef-9a0d-dc009b5f19dd.png)

## Edit Kontak

![Screenshot (50)](https://user-images.githubusercontent.com/115637858/213209642-94599c46-d59f-4148-9eb9-5ea2bb132b6a.png)

## Hapus Kontak

![Screenshot (49)](https://user-images.githubusercontent.com/115637858/213210145-17b96494-569a-4e13-90b0-dd6fab9f83cb.png)

## Cari Kontak

![Screenshot (51)](https://user-images.githubusercontent.com/115637858/213210616-74d29fa4-7091-4053-9b2b-8e6d5f4525a7.png)

## Exit

![Screenshot (53)](https://user-images.githubusercontent.com/115637858/213211294-c0d7b658-75d2-45eb-ab00-e2fd1036d181.png)

# Thank You :)