---
sidebar_position: 2
---

# 2. Blog Detail using PostgreSQL Query

import useBaseUrl from '@docusaurus/useBaseUrl';

Hal pertama yang kita lakukan adalah menambahkan id blog pada file `blog.hbs` bagian `tag anchor` title. Hal ini bertujuan untuk nantinya pada route detail dapat menerima id blog mana yang akan ditampilkan detailnya.

<a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-result-chapter-2/blob/day5-1.blog-detail/views/blog.hbs">
Contoh code
</a>

<br />
<br />

```html title=blog.hbs {12-14}

{{#each blogs}}
  <div class="blog-list-item">
    <div class="blog-image">
      <img src="/public/assets/blog-img.png" alt="Pasar Coding di Indonesia Dinilai Masih Menjanjikan" />
    </div>
    <div class="blog-content">
      <div class="button-group">
        <a class="btn-edit">Edit Post</a>
        <a href="/delete-blog/{{@index}}" class="btn-post">Delete Blog</a>
      </div>
      <h1>
        <a href="/blog/{{this.id}}" target="_blank">{{this.title}}</a>
      </h1>
      <div class="detail-blog-content">
        {{this.post_date}} | {{this.author}}
      </div>
      <p>{{this.content}}</p>
    </div>
  </div>
{{/each}}
```
Pada pertemuan kedua kita telah membuat sebuah route dengan endpoint `blog/:id` untuk menampilkan detail dari sebauah postingan blog. Hanya saja pada pertemuan tersebut kita masih menggunakan data statis dalam menampilkan detail postingan. Kali ini kita akan menggunakan data yang berasal dari database, lalu menampilkannya berdasarkan `id` menggunakan `query SELECT`.

<a class="btn-example-code" href="https://github.com/demo-dumbways/ebook-code-result-chapter-2/blob/day5-1.blog-detail/api/index.js">
Contoh code
</a>

<br />
<br />

```js title=index.js {3-17}
// this code below endpoint app.get('/blog', ......

app.get('/blog/:id', function (req, res) {
    const blogId = req.params.id

    setHeader(res)
    db.connect((err, client, done) => {
        if (err) throw err

        client.query(`SELECT * FROM blog WHERE id = ${id}`, function (err, result) {
            done()
            if (err) throw err

            res.render('blog-detail', { isLogin: isLogin, blog: result.rows[0] })
        })
    })
})

app.get('/add-blog', function (req, res) {
    setHeader(res)
    res.render("form-blog")
})
```

Mengambil index yang dikirim, kita gunakan `req.params.index`, jadi index tersebut tersimpan didalam `req.params`. Selanjutnya id yang kita dapatkan akan menjadi kondisi pada query yang dijalankan. Kondisi yang dimaksud yakni hanya menampilkan data blog yang id nya sama dengan id yang dikirimkan

<img alt="image1" src={useBaseUrl('img/docs/image-5-2.png')} height="400px"/>

<br />
<br />

<div>
<a class="btn-demo" href="https://personal-web-chapter-2.herokuapp.com/detail-blog/8">
Demo
</a>
</div>