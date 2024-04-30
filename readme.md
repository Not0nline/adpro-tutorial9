1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

- Unary RPCs: Ini seperti panggilan fungsi normal di mana klien mengirimkan satu permintaan ke server dan mendapatkan satu respons kembali. Cocok untuk skenario seperti sistem autentikasi/otorisasi atau pengiriman form.
- Server Streaming RPCs: Dalam skenario ini, klien mengirimkan permintaan ke server dan mendapatkan aliran untuk membaca sejumlah pesan kembali. Server akan mengirimkan data berulang kali melalui satu koneksi yang sama, sehingga data yang dikirimkan adalah potongan-potongan data yang membentuk data yang lebih besar. Ini berguna ketika server perlu mengirimkan data dalam jumlah yang sangat besar berkali-kali, misalnya harga saham, berita, atau dataset yang besar.
- Bidirectional Streaming RPCs: Kedua belah pihak mengirimkan sejumlah pesan menggunakan aliran baca-tulis. Dua aliran beroperasi secara independen, jadi klien dan server dapat membaca dan menulis dalam urutan apa pun yang mereka suka. Ini cocok untuk skenario di mana server dan klien sering mengirimkan data berulang kali, misalnya pada editing kolaboratif atau game real-time.

<br>

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

- Otentikasi: Setiap potongan data yang dikirimkan oleh gRPC memerlukan validasi otentikasi untuk menjamin keamanannya. Ini berbeda dengan REST di mana hanya diperlukan satu kali validasi untuk setiap permintaan.
- Otorisasi: Setiap potongan data juga memerlukan validasi otorisasi.
- Enkripsi Data: Setiap potongan data yang dikirimkan baik oleh server maupun klien perlu dienkripsi secara terpisah untuk menjamin privasi data yang dikirim.

<br>

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

- Dalam pembuatan sistem dengan streaming dua arah dan gRPC, kita perlu berhati-hati untuk mencegah kondisi balapan dan memastikan sinkronisasi data. Selain itu, koneksi yang tetap aktif dalam waktu lama dapat menyulitkan penyeimbangan beban. Hal ini karena bisa jadi terbentuk banyak koneksi yang tetap aktif dalam waktu lama, yang tidak bisa diputuskan karena gRPC, sehingga dapat membebani server.

<br>

4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?

- Keuntungan:
    - Asinkronitas yang mempercepat proses pengolahan data.
    - Integrasi yang mudah dengan modul-modul lain dari tokio.
- Kerugian:
    - Kompleksitas yang ditimbulkan oleh pemrograman asinkron.
    - Autentikasi dan otorisasi perlu dilakukan untuk setiap data.

<br>

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

-  Rust gRPC menyediakan cara untuk meningkatkan pemeliharaan dan ekstensibilitas kode. Dengan menggunakan proto, akan dibuat semacam antarmuka yang dapat diimplementasikan oleh suatu kelas di Rust. Hal ini memudahkan ekstensibilitas kode karena perubahan dan penambahan fitur dapat dilakukan dengan lebih mudah.

<br>


6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

- Untuk menangani logika pemrosesan pembayaran yang lebih kompleks, bisa dipertimbangkan untuk mengubahnya menjadi server streaming dan bukan unary. Dengan demikian, pengiriman data yang kompleks dapat dilakukan dengan lebih cepat dan mengurangi overhead pembuatan koneksi antara klien dan server.

<br>

7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

- Dengan menggunakan gRPC, kita tidak perlu lagi memikirkan bagaimana suatu modul dapat diakses dengan metode HTTP. gRPC akan secara otomatis menghubungkan pemanggilan metode yang kita inginkan karena sudah ada pemahaman yang sama melalui file proto. Dengan demikian, klien seolah-olah dapat memanggil fungsi dari server. Hal ini memudahkan konektivitas dan operasi antara teknologi, platform, dan sistem terdistribusi.

<br>

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

- Keuntungan: HTTP/2 memberikan kemampuan di mana banyak permintaan dan respons dapat dilakukan dalam satu koneksi tanpa perlu memutus koneksi.
- Kerugian: Karena HTTP/2 bisa mempertahankan satu koneksi untuk banyak permintaan dan respons, maka akan diperlukan biaya overhead yang lebih tinggi baik untuk kinerja maupun memori. Untuk pengiriman data yang kecil, biaya ini bisa jadi lebih besar dibandingkan dengan HTTP/1.1, meskipun untuk data yang banyak, HTTP/2 lebih efisien. user paraphrase this for number 9 Keuntungan dari gRPC adalah gRPC dapat melakukan streaming data secara real time dan juga dapat melakukan komunikasi dua arah yang tidak bisa dilakukan oleh REST API. Sedangkan kekurangan dari gRPC adalah karena gRPC menggunakan HTTP/2 yang mempertahankan koneksi, maka akan memakan biaya overhead yang lebih tinggi baik untuk performance maupun memory.

<br>

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

- Dalam hal komunikasi real-time dan responsivitas, gRPC memiliki keunggulan dibandingkan REST APIs. Untuk REST APIs, setiap pengiriman permintaan memerlukan pembuatan koneksi baru dari klien ke server. Sebaliknya, gRPC hanya memerlukan satu koneksi sampai semua permintaan selesai. Hal ini membuat gRPC lebih optimal untuk komunikasi real-time dan responsivitas karena kecepatannya yang lebih baik, sehingga lebih “real time”.

<br>

10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

- Dengan pendekatan Protocol Buffers dalam gRPC, klien tidak perlu khawatir apakah JSON yang dikirimkan memang berisi informasi yang tepat. Hal ini karena gRPC dan Protocol Buffers-nya akan membuat antarmuka yang akan menghasilkan kode klien dan server, dan saat runtime akan melakukan serialisasi dan deserialisasi pesan yang masuk dan keluar secara otomatis. Dengan demikian, setiap data yang masuk dan keluar dapat dipastikan dapat digunakan dan tidak salah tipe. Sebaliknya, jika dibandingkan dengan JSON di REST APIs, sangat mungkin untuk mengirimkan data yang berisi informasi yang tidak dapat digunakan oleh salah satu pihak karena tidak ada batasan mengenai apa yang perlu dikirim.
