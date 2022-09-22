---
sidebar_position: 3
---

# 3. Blog Post using PostgreSQL Query

import useBaseUrl from '@docusaurus/useBaseUrl';

Pada pertemuan kedua kita telah membuat sebuah route menggunakan metode `POST` dengan nama endpoint `blog` untuk menyimpan data inputan dari formulir blog. Hanya saja pada pertemuan tersebut kita masih sebatas menyimpan data pada variabel. Kali ini kita akan menyimpan data kedalam database menggunakan `query INSERT`.

<a class="btn-example-code" href="">
Contoh code
</a>

<br />
<br />

```go title=main.go {13-18}
// continuation this code same like before
// this code below func formBlog(w http.ResponseWriter, r *http.Request) { .......

func addBlog(w http.ResponseWriter, r *http.Request) {
	err := r.ParseForm()
	if err != nil {
		log.Fatal(err)
	}

	title := r.PostForm.Get("title")
	content := r.PostForm.Get("content")

	_, err = connection.Conn.Exec(context.Background(), "INSERT INTO blog(title, content,image) VALUES ($1,$2,'image.png')", title, content)
	if err != nil {
		w.WriteHeader(http.StatusInternalServerError)
		w.Write([]byte("message : " + err.Error()))
		return
	}

	http.Redirect(w, r, "/blog", http.StatusMovedPermanently)
}

// continuation this code same like before
```

Pada saat memasukkan data inputan yang kita terima kedalam database, maka kita perlu mespesifikkan column mana saja yang akan kita inputkan. Pada study case kali ini, kita belum menangani terkait `file upload`. Hal tersebut belum dilakukan karena membutuhkan package tambahan yang nantinya akan kita bahas pada pertemuan Day 7 terkait `PostgreSQL Relation and File Upload in Golang`. Sehingga pada case kali ini kita masih menggunakan data statis untuk mengisi data pada colum `image`.

<img alt="image1" src={useBaseUrl('img/docs/image-5-3.png')} height="400px"/>

<br />
<br />

<div>
<a class="btn-demo" href="">
Demo
</a>
</div>
