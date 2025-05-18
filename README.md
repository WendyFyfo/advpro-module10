# MODULE 10 - Aynchronous Programming
Muhammad Wendy Fyfo Anggara - 2306223906


## Experiment 1.2: Understanding how it works
![Experiment 1.2](/image/image.png)

Alasan `println!(Wendy's Computer: Wendid!)` dicetak pertama adalah kerana merupakan baris sinkronus sebelum `.await`, sehingga langsung dijalankan setelah `spawner.spawn()` dipanggil.

Alasan `println!(Wendy's Computer: Wowdy!)` dicetak berikutnya karena merupakan bagian dari blok async yang dijalankan oleh executor. baris ini muncul sebelum `await TimerFuture::new(...)`, jadi akan dijalankan saat task pertama kali dipolling.

Setelah await, program menunggu 2 detik di thread lain, lalu melanjutkan dan menjalankan `println!(Wendy's Computer: Wowdy!)`. Executor menjalankan kembali task setelah timer selesai.

