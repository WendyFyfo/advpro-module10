# MODULE 10 - Aynchronous Programming
Muhammad Wendy Fyfo Anggara - 2306223906


## Experiment 1.2: Understanding how it works
![Experiment 1.2](/image/image.png)

Alasan `println!(Wendy's Computer: Wendid!)` dicetak pertama adalah kerana merupakan baris sinkronus sebelum `.await`, sehingga langsung dijalankan setelah `spawner.spawn()` dipanggil.

Alasan `println!(Wendy's Computer: Wowdy!)` dicetak berikutnya karena merupakan bagian dari blok async yang dijalankan oleh executor. baris ini muncul sebelum `await TimerFuture::new(...)`, jadi akan dijalankan saat task pertama kali dipolling.

Setelah await, program menunggu 2 detik di thread lain, lalu melanjutkan dan menjalankan `println!(Wendy's Computer: Wowdy!)`. Executor menjalankan kembali task setelah timer selesai.

## Experiment 1.3: Multiple Spawn and Removing Drop

### Multiple spawn
![Experiment 1.3 multiple spawn](/image/image%20copy.png)
Dengna menambahkan 2 panggilan spawner.spawn, terlihat bahwa masing -masing mencetak "Wowdy!" secara berurutan, sesuai dengan urutan panggilan. Setelah menunggu 2 detik dengan `TimerFuture`, semua task mencetak "Wendone!". Urutan munculnya "Wendone!" berbeda-beda karena tergantung urutan task yang dieksekusi kembali oleh excecutor. Perbedaan urutan ini muncul karena "TimerFuture" menjalankan `sleep` dalam tiga thread terpisah. Meskipun durasi sleepnya sama, siapa yang selesai lebih dulu bergantung pada timing sistem dan kondisi race antar thread.

### Removing drop
![Experiment 1.3 multiple spawn](/image/image%20copy%202.png)
Dengan menghapus baris `drop(spawner)`, program menjadi macet dan tidak selesai karena `Executor::run()` terus membaca task dari `ready_queue` pakai `recv()` yang akan menunggu terus sampai semua `Spawner` ditutup. 