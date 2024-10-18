Nama : M Afnan Baihaqi
Nim : H1D022080
Shift Asli : A
Shift Baru : B
Penjelasan Source Code :
Kelas StatusForm dirancang untuk menangani operasi CRUD (Create, Read, Update, Delete) 
pada status peminjaman.
1. Create (Simpan): Fungsi simpan digunakan untuk menambahkan status baru. Ketika 
tombol submit ditekan dan form divalidasi, data dari input field diambil dan objek 
Status baru dibuat. Fungsi StatusBloc.addStatus dipanggil untuk menyimpan data ke 
dalam sistem. Jika berhasil, pengguna diarahkan kembali ke halaman StatusPage, dan 
dialog sukses ditampilkan. Jika terjadi kesalahan, dialog peringatan muncul.

void simpan() {
 setState(() {
 _isLoading = true;
 });
 Status newStatus = Status(
 id: null,
 availability: _availabilityController.text,
 borrower_name: _borrowerNameController.text,
 due_days: int.tryParse(_dueDaysController.text),
 );
 StatusBloc.addStatus(status: newStatus).then((value) {
 Navigator.of(context).pushReplacement(
 MaterialPageRoute(
 builder: (BuildContext context) => const StatusPage(),
 ),
 );
 showDialog(
 context: context,
 builder: (BuildContext context) => const SuccessDialog(
 description: "Data berhasil disimpan",
 ),
 );
 }).catchError((error) {
 showDialog(
 context: context,
 builder: (BuildContext context) => const WarningDialog(
 description: "Simpan gagal, silahkan coba lagi",
 ),
 );
 }).whenComplete(() {
 setState(() {
 _isLoading = false;
 });
 });
}

2. Read (Baca): Proses membaca data terjadi saat widget StatusForm pertama kali 
diinisialisasi. Jika parameter status tidak null, kontroler input diisi dengan data yang 
ada, dan judul serta teks tombol diubah menjadi mencerminkan tindakan pembaruan. 
Hal ini memberikan pengguna konteks tentang status yang sedang diedit.

body: FutureBuilder<List>(
 future: StatusBloc.getAllStatus(), // Mengambil data status dari 
bloc
 builder: (context, snapshot) {
 if (snapshot.hasError) print(snapshot.error);
 return snapshot.hasData
 ? ListStatus(list: snapshot.data) // Menampilkan daftar 
status
 : const Center(
 child: CircularProgressIndicator(),
 );
 },
 ),
 );
 }
}

3. Update (Ubah): Fungsi ubah berfungsi untuk memperbarui status yang sudah ada. 
Sama seperti fungsi simpan, data dari input field diambil untuk membuat objek Status
baru dengan ID yang sama. Fungsi StatusBloc.updateStatus kemudian dipanggil untuk 
memperbarui data dalam sistem. Setelah proses berhasil, pengguna diarahkan kembali 
dengan dialog yang menunjukkan status operasi.

void ubah() {
 setState(() {
 _isLoading = true;
 });
 Status updatedStatus = Status(
 id: widget.status!.id,
 availability: _availabilityController.text,
 borrower_name: _borrowerNameController.text,
 due_days: int.tryParse(_dueDaysController.text),
 );
 StatusBloc.updateStatus(status: updatedStatus).then((value) {
 Navigator.of(context).pushReplacement(
 MaterialPageRoute(
 builder: (BuildContext context) => const StatusPage(),
 ),
 );
 showDialog(
 context: context,
 builder: (BuildContext context) => const SuccessDialog(
 description: "Data berhasil diubah",
 ),
 );
 }).catchError((error) {
 showDialog(
 context: context,
 builder: (BuildContext context) => const WarningDialog(
 description: "Permintaan ubah data gagal, silahkan coba 
lagi",
 ),
 );
 }).whenComplete(() {
 setState(() {
 _isLoading = false;
 });
 });
 }
}

4. Delete (Hapus): Meskipun tidak terdapat fungsi hapus dalam kode ini, fungsi tersebut 
dapat ditambahkan dalam konteks yang lebih luas dengan memanggil metode dari 
StatusBloc untuk menghapus status berdasarkan ID. Biasanya, penghapusan akan 
memerlukan konfirmasi dari pengguna sebelum melakukan tindakan.
