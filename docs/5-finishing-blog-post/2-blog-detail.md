---
sidebar_position: 2
---

# 2. Blog Detail using PostgreSQL Query

import useBaseUrl from '@docusaurus/useBaseUrl';

Hal pertama yang kita lakukan adalah menambahkan `id` blog pada file `blog.html` bagian `tag anchor` title. Hal ini bertujuan untuk nantinya pada route detail dapat menerima id blog mana yang akan ditampilkan detailnya.

<a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-result-chapter-2-golang/blob/day5-1-blog-detail/views/blog.html">
Contoh code
</a>

<br />
<br />

```html title=blog.html {62}
<html>
  <head>
    <title>{{.Data.Title}}</title>
    <link rel="stylesheet" href="/public/style.css" />
    <!-- linking boostrap css cdn  -->
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3"
      crossorigin="anonymous"
    />
  </head>

  <body>
    <!-- NavBar -->
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="container-lg">
        <a class="navbar-brand me-5" href="/home">
          <img src="/public/assets/logo.png" alt="logo" />
        </a>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav">
            <li class="nav-item">
              <a class="nav-link" href="/home">Home</a>
            </li>
            <li class="nav-item">
              <a href="/blog" class="nav-link list-active">Blog</a>
            </li>
          </ul>
        </div>
        <div class="d-flex contact-me">
          <a href="/contact-me"> Contact Me </a>
        </div>
      </div>
    </nav>

    <!-- Blog list -->
    <div id="contents" class="blog-list">
      <!-- conditional post blog -->
      {{if .Data.IsLogin}}
      <div class="button-group w-100">
        <a href="/add-blog" class="btn-post">Add New Blog</a>
      </div>
      {{end}}
      <!-- dynamic content would be here -->
      {{range $index, $data := .Blogs}}
      <div class="blog-list-item">
        <div class="blog-image">
          <img
            src="/public/assets/blog-img.png"
            alt="Pasar Coding di Indonesia Dinilai Masih Menjanjikan"
          />
        </div>
        <div class="blog-content">
          {{if $.Data.IsLogin}}
          <div class="button-group">
            <a class="btn-edit">Edit Post</a>
            <a class="btn-post" href="/delete-blog/{{$index}}">Delete Blog</a>
          </div>
          {{end}}
          <h1>
            <a href="/blog/{{$data.Id}}" target="_blank"> {{$data.Title}} </a>
          </h1>
          <div class="detail-blog-content">
            {{$data.Post_date}} | {{$data.Author}}
          </div>
          <p>{{$data.Content}}</p>
        </div>
      </div>
      {{end}}
    </div>
  </body>
</html>
```

Pada pertemuan kedua kita telah membuat sebuah route dengan endpoint `blog/{id}` untuk menampilkan detail dari sebauah postingan blog. Hanya saja pada pertemuan tersebut kita masih menggunakan data statis dalam menampilkan detail postingan. Kali ini kita akan menggunakan data yang berasal dari database, lalu menampilkannya berdasarkan `id` menggunakan `query SELECT`.

<a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-result-chapter-2-golang/blob/day5-1-blog-detail/main.go">
Contoh code
</a>

<br />
<br />

```go title="main.go" {15-30}
// this code below func blogs(w http.ResponseWriter, r *http.Request) { ..........

func blogDetail(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("Content-Type", "text/html; charset=utf-8")

	id, _ := strconv.Atoi(mux.Vars(r)["id"])

	var tmpl, err = template.ParseFiles("views/blog-detail.html")
	if err != nil {
		w.WriteHeader(http.StatusInternalServerError)
		w.Write([]byte("message : " + err.Error()))
		return
	}

	BlogDetail := Blog{}
	err = connection.Conn.QueryRow(context.Background(), "SELECT id, title, image, content, post_date FROM tb_blog WHERE id=$1", id).Scan(
		&BlogDetail.Id, &BlogDetail.Title, &BlogDetail.Image, &BlogDetail.Content, &BlogDetail.Post_date)
	if err != nil {
		w.WriteHeader(http.StatusInternalServerError)
		w.Write([]byte("message : " + err.Error()))
		return
	}

	BlogDetail.Author = "Ilham Fathullah"
	BlogDetail.Format_date = BlogDetail.Post_date.Format("2 January 2006")

	resp := map[string]interface{}{
		"Data": Data,
		"Blog": BlogDetail,
	}

	BlogDetail.Author = "Ilham Fathullah"
	BlogDetail.Format_date = BlogDetail.Post_date.Format("2 January 2006")

	resp := map[string]interface{}{
		"Data": Data,
		"Blog": BlogDetail,
	}

	w.WriteHeader(http.StatusOK)
	tmpl.Execute(w, resp)
}

func formBlog(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("Content-Type", "text/html; charset=utf-8")

	var tmpl, err = template.ParseFiles("views/form-blog.html")
	if err != nil {
		w.WriteHeader(http.StatusInternalServerError)
		w.Write([]byte("message : " + err.Error()))
		return
	}

	w.WriteHeader(http.StatusOK)
	tmpl.Execute(w, Data)
}

// continuation this code same like before
```

Mengambil id yang dikirim, kita gunakan `mux.Vars(r)["id"]`, jadi id tersebut tersimpan didalam `mux.Vars(r)`. Selanjutnya id yang kita dapatkan akan menjadi kondisi pada query yang dijalankan. Kondisi yang dimaksud yakni hanya menampilkan data blog yang id nya sama dengan id yang dikirimkan

<img alt="image1" src={useBaseUrl('img/docs/image-5-2.png')} height="400px"/>

<br />
<br />

<div>
<a class="btn-demo" href="">
Demo
</a>
</div>
