---
sidebar_position: 4
---

# 4. Blog Delete using PostgreSQL Query

import useBaseUrl from '@docusaurus/useBaseUrl';

Hal pertama yang kita lakukan adalah menambahkan id blog yang akan dihapus pada file `blog.html` bagian `tag anchor` button delete. Hal ini bertujuan untuk nantinya pada route delete dapat menerima id blog mana yang akan hapus dari database.

<a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-result-chapter-2-golang/blob/day5-3-blog-delete/views/blog.html">
Contoh code
</a>

<br />
<br />

```html title=blog.html {5}
<div class="blog-content">
  {{if $.Data.IsLogin}}
  <div class="button-group">
    <a class="btn-edit">Edit Post</a>
    <a class="btn-post" href="/delete-blog/{{$data.Id}}">Delete Blog</a>
  </div>
  {{end}}
  <h1>
    <a href="/blog/{{$data.Id}}" target="_blank"> {{$data.Title}} </a>
  </h1>
  <div class="detail-blog-content">{{$data.Post_date}} | {{$data.Author}}</div>
  <p>{{$data.Content}}</p>
</div>
```

Pada endpoint `delete-blog/{id}` kita akan melakukan proses menghapus data blog, sesuai dengan tujuannya yakni menghapus data blog dari database. Query yang akan digunakan untuk menghapus data yakni `DELETE`.

<a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-result-chapter-2-golang/blob/day5-3-blog-delete/main.go">
Contoh code
</a>

<br />
<br />

```go title=main.go {9-14}
// continuation this code same like before
// this code below func addBlog(w http.ResponseWriter, r *http.Request) { .....

func deleteBlog(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("Content-Type", "text/html; charset=utf-8")

	id, _ := strconv.Atoi(mux.Vars(r)["id"])

	_, err := connection.Conn.Exec(context.Background(), "DELETE FROM tb_blog WHERE id=$1", id)
	if err != nil {
		w.WriteHeader(http.StatusInternalServerError)
		w.Write([]byte("message : " + err.Error()))
		return
	}

	http.Redirect(w, r, "/blog", http.StatusMovedPermanently)
}

func contactMe(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("Content-Type", "text/html; charset=utf-8")

	var tmpl, err = template.ParseFiles("views/contact.html")
	if err != nil {
		w.WriteHeader(http.StatusInternalServerError)
		w.Write([]byte("message : " + err.Error()))
		return
	}

	w.WriteHeader(http.StatusOK)
	tmpl.Execute(w, Data)
}
```

Mengambil id yang dikirim, kita gunakan `mux.Vars(r)["id"]`, jadi id tersebut tersimpan didalam `mux.Vars(r)`. Selanjutnya id yang kita dapatkan akan menjadi kondisi pada query yang dijalankan. Kondisi yang dimaksud yakni hanya menghapus data blog yang id nya sama dengan id yang dikirimkan.

Sebenarnya penulisan query yang digunakan bisa kita `simpan kedalam sebuah variabel` terlebih dahulu seperti endpoint delete ini, ataupun `langsung dituliskan` seperti yang dituliskan pada endpoint detail blog dan blog post.

<img alt="image1" src={useBaseUrl('img/docs/image-5-5.png')} height="400px"/>

<br />
<br />

<div>
<a class="btn-demo" href="">
Demo
</a>
</div>
